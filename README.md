# Event Management API

A simple RESTful API built with Flask that lets you create, read, update, and
delete (CRUD) events. Data is stored in memory (a Python list), so it resets
whenever the server restarts — there's no real database involved.

## Purpose

This API simulates a small event management system. It demonstrates:

- Route design using `@app.route()` with different HTTP methods
- Reading JSON request bodies with `request.get_json()`
- Returning JSON responses with `jsonify()`
- Using meaningful HTTP status codes (200, 201, 400, 404)

## Running the API

```bash
python app.py
```

The server runs at `http://localhost:5000`.

## Routes

| Method | Route            | Description                     |
|--------|------------------|----------------------------------|
| GET    | `/`              | Welcome message                 |
| GET    | `/events`        | List all events                 |
| POST   | `/events`        | Create a new event              |
| PATCH  | `/events/<id>`   | Update an event's title         |
| DELETE | `/events/<id>`   | Delete an event                 |

## Example Requests & Responses

### GET `/`
**Response — 200 OK**
```json
{ "message": "Welcome to the Event Management API" }
```

### GET `/events`
**Response — 200 OK**
```json
[
  { "id": 1, "title": "Tech Meetup" },
  { "id": 2, "title": "Python Workshop" }
]
```

### POST `/events`
**Request body**
```json
{ "title": "Hackathon" }
```
**Response — 201 Created**
```json
{ "id": 3, "title": "Hackathon" }
```
**Missing title — 400 Bad Request**
```json
{ "error": "Missing required field: title" }
```

### PATCH `/events/1`
**Request body**
```json
{ "title": "Hackathon 2025" }
```
**Response — 200 OK**
```json
{ "id": 1, "title": "Hackathon 2025" }
```
**Event not found — 404 Not Found**
```json
{ "error": "Event with id 1 not found" }
```

### DELETE `/events/2`
**Response — 200 OK**
```json
{ "message": "Event 2 deleted successfully" }
```
**Event not found — 404 Not Found**
```json
{ "error": "Event with id 2 not found" }
```

## Best Practices Followed

- Routes use plural nouns (`/events`) rather than verbs.
- Each route function does one job and stays easy to read.
- Status codes reflect the outcome: `201` for creation, `200` for successful
  reads/updates/deletes, `400` for bad input, `404` for missing resources.
- All responses go through `jsonify()` for consistent JSON formatting.
- A shared `find_event()` helper avoids repeating the lookup logic across
  the PATCH and DELETE routes.

## Notes on Scale

This app keeps everything in a single file and an in-memory list, which is
fine for a lab but won't scale. A production version would move data into a
real database and split routes/models into separate modules.