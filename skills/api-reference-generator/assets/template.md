# API Endpoint Documentation Template

This template is used by the `api-reference-generator` skill for each endpoint entry. Replace all `{{placeholder}}` values. Remove sections marked as optional if they do not apply.

---

### `{{METHOD}} {{/api/resource/path}}`

{{One sentence describing what this endpoint does and why a caller would use it.}}

**Authentication:** {{Required — Bearer token in Authorization header | Required — session cookie | None}}  
**Rate limit:** {{e.g. 100 req/min per IP | Not rate limited | Unknown}}

---

#### Request

**Path Parameters** *(omit section if none)*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `{{param}}` | `string` | Yes | {{Description}} |

**Query Parameters** *(omit section if none)*

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `{{param}}` | `string` | No | `{{default}}` | {{Description}} |

**Request Headers** *(omit section if only standard headers)*

| Header | Required | Description |
|---|---|---|
| `Authorization` | Yes | `Bearer <token>` |
| `{{Header-Name}}` | No | {{Description}} |

**Request Body** *(omit section for GET/DELETE with no body)*

```json
{
  "{{field}}": "{{example value}}",
  "{{field2}}": 123
}
```

TypeScript interface:
```ts
interface {{RequestBodyType}} {
  {{field}}: string;
  {{field2}}: number;
  {{optionalField}}?: boolean;
}
```

---

#### Responses

**`200 OK`** *(or appropriate 2xx status — change heading accordingly)*

```json
{
  "{{field}}": "{{example value}}",
  "{{field2}}": []
}
```

TypeScript interface:
```ts
interface {{ResponseType}} {
  {{field}}: string;
  {{field2}}: {{ChildType}}[];
}
```

**`400 Bad Request`**

Returned when: {{describe the condition — e.g. "required fields are missing or validation fails"}}

```json
{
  "error": "Bad Request",
  "message": "{{field}} is required"
}
```

**`401 Unauthorized`** *(omit if route is public)*

Returned when: the request is missing a valid authentication credential.

```json
{
  "error": "Unauthorized",
  "message": "Authentication required"
}
```

**`403 Forbidden`** *(omit if no authorization checks beyond authentication)*

Returned when: the authenticated user does not have permission to perform this action.

```json
{
  "error": "Forbidden",
  "message": "Insufficient permissions"
}
```

**`404 Not Found`** *(omit if endpoint cannot return 404)*

Returned when: {{describe the condition — e.g. "the requested resource does not exist"}}

```json
{
  "error": "Not Found",
  "message": "{{Resource}} not found"
}
```

**`500 Internal Server Error`**

Returned when: an unexpected server-side error occurs.

```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}
```

---

#### Example Request

```bash
curl -X {{METHOD}} "https://yourdomain.com{{/api/resource/path}}" \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{{request body JSON or omit -d for GET}}'
```

#### Example Response

```json
{{full example success response body}}
```

---

#### Notes *(omit section if none)*

- {{Any important behavioral notes, caveats, side effects, or deprecation warnings}}
