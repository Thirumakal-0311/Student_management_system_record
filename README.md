# Student Management System — CRUD Web Application

A complete Create / Read / Update / Delete web application in a single HTML file.
Built with React 18, plain CSS, and browser storage as the data layer.

 

## Running it

Download `index.html` and double-click it. No installation, no server, no build step.

## What it does

| Operation | How to use it | What happens |
|---|---|---|
| Create | Fill the form, click "Add student" | Validated, stored, row appears in the table |
| Read | Page load, search box, department/year filters, sort menu | Matching records are listed |
| Update | Click "Edit" on a row, change values, click "Save changes" | Record is updated, row refreshes |
| Delete | Click "Delete", confirm in the dialog | Record is removed |

## How it maps to the standard three-tier architecture

```
React components  →  studentApi (data layer)  →  browser storage
   (presentation)      (validation + CRUD)         (persistence)
```

`studentApi` in `index.html` exposes `list`, `retrieve`, `create`, `update`,
`remove` and `stats`, matching a REST backend one for one:

| Method | Equivalent endpoint |
|---|---|
| `studentApi.list()` | `GET /api/students/` |
| `studentApi.retrieve(id)` | `GET /api/students/{id}/` |
| `studentApi.create(data)` | `POST /api/students/` |
| `studentApi.update(id, data)` | `PUT /api/students/{id}/` |
| `studentApi.remove(id)` | `DELETE /api/students/{id}/` |
| `studentApi.stats()` | `GET /api/students/stats/` |

To move to a real Django REST Framework or Spring Boot backend, replace the
bodies of those six methods with `fetch` or `axios` calls. Nothing else changes.

## Validation

Every rule runs twice — once in the form for immediate feedback, once in the
data layer so invalid records cannot be stored even if the form is bypassed.

| Field | Rule |
|---|---|
| Roll number | Required, 4–15 letters or digits, must be unique |
| Full name | Required, at least 3 characters, letters and spaces only |
| Email | Required, valid format, must be unique |
| Mobile | 10 digits beginning with 6–9 |
| Department | One of CSE, IT, ECE, EEE, MECH, CIVIL |
| Year | 1 to 4 |
| CGPA | 0.00 to 10.00; must be above 0 from second year onward |
| Date of birth | Optional, must be in the past |

## Data model

| Field | Type | Constraints |
|---|---|---|
| id | number | Primary key, auto increment |
| roll_number | string(15) | Not null, unique |
| full_name | string(100) | Not null |
| email | string(254) | Not null, unique |
| phone | string(10) | Not null |
| department | string(10) | Not null, fixed choice list |
| year_of_study | number | 1–4 |
| cgpa | decimal(4,2) | 0–10 |
| date_of_birth | date | Nullable |
| is_active | boolean | Default true |
| created_at / updated_at | datetime | Set automatically |

## Testing

| # | Check | Expected |
|---|---|---|
| 1 | Submit the empty form | Message under every required field, nothing saved |
| 2 | Enter `abc` as email | "Enter a valid email address." |
| 3 | Reuse an existing roll number | "This roll number is already registered." |
| 4 | Enter CGPA 15 | "CGPA must be between 0 and 10." |
| 5 | Add a valid student | Green notice, new row, counters update |
| 6 | Edit a record and save | Row highlighted while editing, new values shown |
| 7 | Delete and confirm | Row removed; cancel keeps it |
| 8 | Type in the search box | List narrows after a short pause |
| 9 | Filter by department and year | Only matching rows shown |
| 10 | Refresh the page | Records are still there |
| 11 | Resize to 375px | Table stacks, no sideways scrolling |
| 12 | Tab through the form | Visible focus outline on every control |

## Tech stack

React 18 (CDN), Babel Standalone for JSX, CSS custom properties with light and
dark themes, browser `localStorage` for persistence.
