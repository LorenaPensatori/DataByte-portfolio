# Test Data — DataByte (SiGeRU)

**Academic QA evidence | DataByte team | Uruguay, 2026**

> **Source and attribution.** This document is a curated English-language presentation of test data from the DataByte team's academic *Documento Integrador*, section **3.5.4, “Casos de Prueba con Juegos de Datos”** (printed pages 210–326), cross-referenced with the independent *Caja Negra DataByte* document. It does not represent new executions or newly generated test datasets. Both documents are team-project records; no individual execution is attributed here.

## 1. Dataset inventory and relationships

The integrator identifies **125 test cases** (`CP-001`–`CP-125`) with corresponding **125 data-set identifiers** (`JD-001`–`JD-125`).

| Data-set range | Related cases | Purpose |
| --- | --- | --- |
| `JD-001`–`JD-072` | `CP-001`–`CP-072` | Data prepared for use-case scenarios (basic, alternative, and exception flows) |
| `JD-073`–`JD-125` | `CP-073`–`CP-125` | Data linked to the 53 independent black-box cases, `CN-001`–`CN-053` |

In the second group, the numbering maps sequentially: **`JD-073` ↔ `CP-073` ↔ `CN-001`**, through **`JD-125` ↔ `CP-125` ↔ `CN-053`**.

**Meaning of source labels:** `[V]` indicates a valid value, `[I]` indicates an invalid value, and `n/a` indicates that a value is not applicable. The label **“Diseñado” (Designed)** appears on many of the later dataset records: it identifies the dataset's documented status and must **not** be interpreted as proof of a separate execution of those exact values.

## 2. Representative input datasets

The samples below retain non-sensitive values and describe passwords by their role rather than repeating credential-like strings from the original report. Any names, identity numbers, or emails shown here are **source-labeled academic test inputs**, not verified real identities.

### JD-001 / CP-001 — Valid registration

| Field | Recorded input | Classification |
| --- | --- | --- |
| Email | `qa.usuario01@example.com` | Valid |
| First name | `Ana` | Valid |
| Last name | `Pérez` | Valid |
| Identity field (CI) | `12345678` | Valid in source |
| Password | Matching test password, not reproduced | Valid |
| Confirmation | Matches test password | Valid |

**Test intent:** follow the basic registration flow with required inputs populated.  
**Expected:** registration is accepted.  
**Recorded CP observation:** the form was accepted, confirmation appeared, and the request was visible in administration.

### JD-002 / CP-002 — Missing required email

| Field | Recorded input | Classification |
| --- | --- | --- |
| Email | Empty | Invalid |
| First and last name | `Ana`, `Pérez` | Valid |
| Identity field (CI) | `12345678` | Valid in source |
| Password and confirmation | Matching test values, omitted here | Valid |

**Test intent:** isolate required-email validation while leaving the other inputs valid.  
**Expected:** submission is blocked or rejected and no registration is saved.  
**Recorded CP observation:** a required-field validation appeared; no new request was saved.

**Distinction:** this is **not** the same input configuration as `CN-002`, where **all** registration fields were left empty.

### JD-003 / CP-003 — Invalid email input

| Field | Recorded input | Classification |
| --- | --- | --- |
| Email | `VALOR_NO_EXISTENTE` | Invalid |
| Other registration fields | Marked as valid in the source | Valid |

**Test intent:** exercise an exception flow using an invalid email value.  
**Recorded CP observation:** the input was rejected and existing data was preserved.

**Documentation limitation:** the source describes an “invalid entry / nonexistent reference” scenario, but an invalid email string alone does not establish a nonexistent database reference.

### JD-073 / CP-073 / CN-001 — Registration: original black-box reference

The integrator lists valid inputs similar to JD-001, including `qa.usuario01@example.com`, `Ana`, `Pérez`, and `12345678`. The independent `CN-001` record, however, specifies only that the registration values were valid; it does **not** independently establish that the exact JD-073 values were used in that historic execution.

- **Expected (CN):** registration request is recorded and visible to administration.
- **Recorded actual (CN):** “Usuario creado correctamente”; request visible in administration.
- **Original CN state:** `Aprobado ID`.

### JD-074 / CP-074 / CN-002 — All registration fields empty

- **Original CN input:** *all fields empty*.
- **Expected:** reject submission and indicate mandatory fields.
- **Recorded CN actual:** “El correo es obligatorio.”; no request created.
- **Integrator JD-074 values:** “CI existente en BD de pruebas”, “Rol Inspector Municipal”, and an empty “Campo objetivo”.

**Documentation discrepancy:** the JD-074 data table **does not reproduce the all-empty input described by CN-002**. Do not present JD-074's table as the verified data used during the CN-002 execution.

### JD-075 / CP-075 / CN-003 — Invalid email format

| Field | Integrator data | Classification |
| --- | --- | --- |
| Email | `pruebacorreo.com` | Invalid |
| CI | “CI existente en BD de pruebas” | Described as valid, exact value absent |
| Role | `Inspector Municipal` | Marked valid in JD; context requires review |

- **Expected (CN):** reject malformed email and create no registration request.
- **Recorded actual (CN):** “Ingresá un correo electrónico válido.”; no request in administration.
- **Limitation:** the CN says the remaining fields were valid but does not provide their exact values.

### JD-079 / CP-079 / CN-007 — Missing password confirmation

- **Original CN condition:** confirmation field left **empty**; other registration fields valid.
- **Recorded actual:** “Debés confirmar la contraseña.”; no account or request created.
- **Integrator discrepancy:** JD-079 lists a populated valid confirmation field *and* a separate empty “Campo objetivo”. This conflicts with the targeted CN condition.

**Treatment:** preserve the conflict; do not silently substitute a corrected dataset or claim the documented JD-079 table was executed.

### JD-104 / CP-104 / CN-032 — Administrator registration with invalid email

- **Integrator email:** `pruebacorreo.com` `[I]`.
- **CN scenario:** registration of an **Administrator**, with otherwise valid fields.
- **Integrator JD table:** includes `Rol Inspector Municipal` `[V]`.

**Documentation discrepancy:** the role shown in the data table does not match the Administrator-registration scenario. The CN source records that registration was blocked; the exact combination of JD-104 values is not independently verified.

### JD-117 / CP-117 — Administrator statistics

- **CN context:** an **Administrator** accesses general statistics.
- **Integrator JD table:** uses `Rol Inspector Municipal` and an existing test identity.

**Documentation discrepancy:** the dataset role conflicts with the case's Administrator precondition. This is a preparation/traceability issue, not a newly observed application bug.

### JD-118 / CP-118 / CN-046 — Municipal Inspector route supervision

| Field or precondition | Integrator record |
| --- | --- |
| Role | Authenticated Municipal Inspector |
| Consultation date | `2026-11-05` `[V]` |
| Route | “Ruta existente asignada” `[V]` (exact route ID not provided) |
| Existing records | Routes and vehicles available to inspect |

**Expected (CN):** view assigned routes and vehicles, and access route registration and truck assignment.  
**Recorded actual (CN):** route information was visible, but the route-registration and truck-assignment options could not be accessed.  
**Original CN state:** **Fail**.

**Follow-up:** no verified fix or retest record is available. The JD status “Diseñado” does not change the independent CN result of **Fail**.

### JD-124 / CP-124 / CN-052 — Waste intake registration

| Field | Integrator input |
| --- | --- |
| Establishment | “ID existente en BD” `[V]` |
| Quantity | `50` `[V]` |
| Waste type | “Tipo habilitado en catálogo” `[V]` |

**Expected:** record an intake entry and make it retrievable.  
**Recorded CN outcome:** successful waste intake, confirmation displayed, and the entry could subsequently be consulted.

The exact establishment ID and catalog value are not provided; this is a **partially parameterized** dataset, not a ready-to-run fixture.

### JD-125 / CP-125 / CN-053 — Crew member views routes

- **Consultation date:** `2026-11-05` `[V]`.
- **Route:** “Ruta existente asignada” `[V]`.
- **Precondition:** authenticated Crew Operator with route consultation access.
- **Expected:** route list appears ordered by date.
- **Recorded CN actual:** routes appeared and were ordered by date.
- **Limitation:** the integrator's specific “Resultado esperado (caja negra)” cell is blank even though expected behavior is described elsewhere in the record.

## 3. Techniques illustrated by the source data

| Technique | Documented example | What it exercises |
| --- | --- | --- |
| Valid input / positive scenario | `JD-001` | Successful registration using required values |
| Missing required field | `JD-002` | Email mandatory-field validation |
| Invalid format | `JD-075` | Email formatting validation |
| Empty-field combination | `CN-002` | Registration when all required fields are blank |
| Invalid confirmation | `CN-007` | Missing password confirmation |
| Role / access preconditions | `CN-022`, `CN-046` | Authorization and feature availability |
| Existing relational data | `JD-124` | Waste intake linked to a pre-existing establishment |

The documents describe **valid and invalid input classes**. They should not be represented as a comprehensive boundary-value analysis unless actual minimum/maximum boundary datasets are identified.

## 4. Reproducibility and evidence limitations

Some source fields are placeholders, such as “CI existente en BD de pruebas”, “ID existente en BD”, “Ruta existente asignada”, or “Tipo habilitado en catálogo”. These require a seeded test environment before they could be reproduced.

For a future rerun, a tester would need to record the environment/build, prepared test records, exact safe input values, execution date, step-by-step observations, evidence, and defect reference. **Those would be requirements for new verification, not facts about the historic executions.**

Additional limitations:

- The independent `CN` records and the integrator's `JD` values are related documents, but their exact inputs do not always match.
- The historical `CN` states record outcomes; `JD` entries marked “Designed” should not be counted as independently executed test cases.
- No separate screenshot/log package or confirmed retest for `CN-046` was supplied. The integrator does contain some Postman screenshots, but those are separate API evidence.
- All artifacts are treated as collaborative DataByte team materials.

## 5. Privacy and security

This public-oriented summary does **not** reproduce original credential-like passwords, administrative secrets, internal host addresses, or full source PDF attachments. Inputs are shown only when they are suitable as demonstrative academic test values. Actual secrets must be rotated and excluded before any source code or raw report is made public.

## Sources retained outside the public repository

- *Documento Integrador — DataByte, Tercera Entrega*, section 3.5.4, printed pp. 210–326.
- *Caja Negra DataByte*, 118 pages, cases `CN-001`–`CN-053`.

These source documents are not published as part of this curated portfolio.
