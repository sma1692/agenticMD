# Contract: `<project>` — `<feature-name>`

- **Status**: `DRAFT` | `LOCKED` | `IMPLEMENTED`
- **Project**: `<project-name>`
- **Frontend Target Path**: `<resolved-fe-path>`
- **Backend Target Path**: `<resolved-be-path>`
- **Created At**: `YYYY-MM-DD`

---

## 1. Feature / Bug Summary
Brief description of the business requirement, user flow, or bug root cause.

---

## 2. Agreed API Contract

### Endpoint 1: `<METHOD> /api/v1/...`
- **Auth / Headers**: `Authorization: Bearer <token>`
- **Query / Path Params**:
  - `id` (`string`, required): Resource identifier
- **Request Body (JSON)**:
```json
{
  "exampleField": "string"
}
```
- **Success Response (`200 OK` / `201 Created`)**:
```json
{
  "data": {
    "id": "string",
    "exampleField": "string"
  }
}
```
- **Error Responses**:
  - `400 Bad Request`: Validation failure
  - `404 Not Found`: Resource missing

---

## 3. Domain Execution Plan

### Backend (`backend-lead`)
- [ ] Model / DB Schema changes:
- [ ] Service / Business logic changes:
- [ ] Controller / Route changes:
- [ ] Verification / Tests:

### Frontend (`frontend-lead`)
- [ ] API Client / Type definitions:
- [ ] State / Hooks changes:
- [ ] UI Components / Views:
- [ ] Verification / Tests:
