# pass.in

Pass.in is an application for **managing participants in in-person events**.

The tool allows the organizer to register an event and open a public registration page.

Registered participants can generate a credential for check-in on the day of the event.

The system will scan the participant's credential to allow entry to the event.

This Project was created during the Next Level Week by [Rocketseat](https://rocketseat.com.br)

## How to run

```
# Prerequisites (Node >=20)

# Clone the repository
$ git clone https://github.com/luccasro/passin

# Install the dependencies
$ npm install

# Create a .env file using .env.sample as a guide

# Seeding
$ npx prisma db seed

# Run the project
$ npm run dev
```

## Requirements

### Functional Requirements

- [x] The organizer should be able to register a new event.
- [x] The organizer should be able to view event data.
- [x] The organizer should be able to view the list of participants.
- [x] The participant should be able to register for an event.
- [x] The participant should be able to view their registration badge.
- [x] The participant should be able to check-in at the event.

### Business Rules

- [x] The participant can only register for an event once.
- [x] The participant can only register for events with available spots.
- [x] The participant can only check-in at an event once.

### Non-functional Requirements

- [x] Check-in at the event will be done through a QRCode.

cd /Users/luccasro/Projetos/externos/nlw-unite-nodejs/

## API Documentation (Swagger)

For API documentation, visit the link: https://nlw-unite-nodejs.onrender.com/docs

## Database

In this application, we will use a relational database (SQL). For development environment, we will use SQLite for its ease of use.

### ERD Diagram

<img src=".github/erd.svg" width="600" alt="ERD Diagram of the database" />

### Database Structure (SQL)

```sql
-- CreateTable
CREATE TABLE "events" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "title" TEXT NOT NULL,
    "details" TEXT,
    "slug" TEXT NOT NULL,
    "maximum_attendees" INTEGER
);

-- CreateTable
CREATE TABLE "attendees" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL,
    "event_id" TEXT NOT NULL,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT "attendees_event_id_fkey" FOREIGN KEY ("event_id") REFERENCES "events" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateTable
CREATE TABLE "check_ins" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "attendeeId" INTEGER NOT NULL,
    CONSTRAINT "check_ins_attendeeId_fkey" FOREIGN KEY ("attendeeId") REFERENCES "attendees" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateIndex
CREATE UNIQUE INDEX "events_slug_key" ON "events"("slug");

-- CreateIndex
CREATE UNIQUE INDEX "attendees_event_id_email_key" ON "attendees"("event_id", "email");

-- CreateIndex
CREATE UNIQUE INDEX "check_ins_attendeeId_key" ON "check_ins"("attendeeId");
```
