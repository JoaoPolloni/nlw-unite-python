# Pass.in - Aplicação de Gestão de Participantes em Eventos

Pass.in é uma aplicação de gestão de participantes em eventos presenciais, projetada para permitir que organizadores criem e gerenciem eventos, enquanto os participantes podem se inscrever, visualizar seus crachás de inscrição e realizar check-in no evento.

## Requisitos Funcionais:

1. O organizador pode cadastrar um novo evento.
2. O organizador pode visualizar dados de um evento.
3. O organizador pode visualizar a lista de participantes.
4. O participante pode se inscrever em um evento.
5. O participante pode visualizar seu crachá de inscrição.
6. O participante pode realizar check-in no evento.

## Regras de Negócio:

1. O participante só pode se inscrever em um evento uma única vez.
2. O participante só pode se inscrever em eventos com vagas disponíveis.
3. O participante só pode realizar check-in em um evento uma única vez.

## Requisitos Não-funcionais:

1. O check-in no evento será realizado através de um QRCode.

## Especificações da API:

### Banco de Dados:

- Utilizamos um banco de dados relacional (SQL), com SQLite para ambiente de desenvolvimento.
- Diagrama ERD especificando a estrutura do banco.

### Estrutura do Banco (SQL):

```sql
CREATE TABLE "events" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "title" TEXT NOT NULL,
    "details" TEXT,
    "slug" TEXT NOT NULL,
    "maximum_attendees" INTEGER
);

CREATE TABLE "attendees" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL,
    "event_id" TEXT NOT NULL,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT "attendees_event_id_fkey" FOREIGN KEY ("event_id") REFERENCES "events" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

CREATE TABLE "check_ins" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "attendeeId" TEXT NOT NULL,
    CONSTRAINT "check_ins_attendeeId_fkey" FOREIGN KEY ("attendeeId") REFERENCES "attendees" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

CREATE UNIQUE INDEX "events_slug_key" ON "events"("slug");
CREATE UNIQUE INDEX "attendees_event_id_email_key" ON "attendees"("event_id", "email");
CREATE UNIQUE INDEX "check_ins_attendeeId_key" ON "check_ins"("attendeeId");
