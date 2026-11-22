# Test Cases — DataByte (SiGeRU)

**Academic QA evidence | DataByte team | Uruguay, 2026**

> **Provenance and scope.** This is a curated, English-language presentation of original team documentation, not a new test run. The DataByte integrator (version 3.0, 6 November 2026), section **3.5.4, “Casos de Prueba con Juegos de Datos”** (printed pages 210–326), contains cases `CP-001`–`CP-125` and linked datasets `JD-001`–`JD-125`. The separate *Caja Negra DataByte* document records cases `CN-001`–`CN-053` (dated 5 November 2026). Results below are **recorded historical observations**, not independently revalidated claims. The original PDF files are not included in this public-facing summary. Work is attributed to the DataByte team as a collaborative academic project.

## 1. Case organization

| Integrator cases | Source grouping | Dataset references | Recorded-result context |
| --- | --- | --- | --- |
| `CP-001`–`CP-072` | 24 use cases (`CU_01` onward), with basic, alternative, and exception scenarios | `JD-001`–`JD-072` | The integrator includes written expected and obtained results; some are generic descriptions rather than independent evidence |
| `CP-073`–`CP-125` | Cases adapted from the separate black-box records | `JD-073`–`JD-125` | Each refers to `CN-001`–`CN-053`, respectively; use the CN records to understand the original observations and recorded status |

**Identification rule:** for the second group, `CP-073 ↔ CN-001 ↔ JD-073`, continuing sequentially through `CP-125 ↔ CN-053 ↔ JD-125`. This is *documentary cross-referencing*, not a statement that a case was executed twice. Related-case references in the first group may cover multiple CN scenarios and should not be interpreted as one-to-one evidence of execution.

**Notation:** `[V]` and `[I]` in the source identify valid and invalid inputs. `Designed` (`Diseñado`) is the status assigned to many JD records; it does **not** mean that an independent execution of that precise dataset was established.

## 2. Representative use-case-derived cases

### CP-001 — User registration: basic flow with valid data

| Field | From the integrator |
| --- | --- |
| Use case | `CU_01 — Registrarse` |
| Dataset | `JD-001` |
| Priority | High |
| Preconditions | User not previously registered |
| Input | `qa.usuario01@example.com`; first name `Ana`; surname `Pérez`; identity field `12345678`; matching password and confirmation (not reproduced here) |
| Procedure | Prepare the data and session; open **Registrarse**; submit JD-001; check the message, interface, and persistence where applicable |
| Expected | The registration completes according to the use case without validation errors |
| Obtained (written in CP-001) | The form was accepted, confirmation appeared, and the registration request became visible in administration |
| Related black-box cases in source | `CN-001`–`CN-010` (a *set* of registration scenarios, not ten executions of CP-001) |

**Evidence note:** `CN-001` describes a closely related successful registration, but the separate black-box record does not specify the literal inputs shown in JD-001. Do not assert they were the same execution.

### CP-002 — User registration: missing mandatory email

| Field | From the integrator |
| --- | --- |
| Use case | `CU_01 — Registrarse` |
| Dataset | `JD-002` |
| Priority | Medium |
| Preconditions | User not previously registered |
| Input | Email empty `[I]`; first name `Ana`, surname `Pérez`, identity field `12345678` and matching passwords `[V]` |
| Procedure | Submit the registration with JD-002 and inspect validation and stored changes |
| Expected | Required-field warning; invalid changes must not be saved |
| Obtained (written in CP-002) | A required-field validation was shown; no new registration was saved |

**Evidence note:** unlike `CN-002` (which leaves *all* fields empty), CP-002 isolates an empty email while other values are supplied. These are distinct test conditions.

### CP-003 — User registration: invalid entry / reference

| Field | From the integrator |
| --- | --- |
| Use case | `CU_01 — Registrarse` |
| Dataset | `JD-003` |
| Priority | Medium |
| Preconditions | User not previously registered |
| Input | The email field contains `VALOR_NO_EXISTENTE` `[I]`; other fields are represented as valid in the source |
| Procedure | Submit the invalid dataset and check validation and record consistency |
| Expected | Reject invalid input and preserve stored data |
| Obtained (written in CP-003) | Invalid data or identifier rejected, warning shown, existing records preserved |

**Evidence limitation:** the original scenario title combines “nonexistent reference” and “invalid entry”; the supplied email value does not by itself establish a nonexistent database reference. This ambiguity is preserved rather than silently corrected.

## 3. Cases linked to the original black-box records

### CP-073 / CN-001 — Registration with valid inputs

- **Dataset:** `JD-073` (marked `Diseñado` in the integrator).
- **Precondition:** the user is not already registered.
- **Input:** the integrator lists a valid email, first name, surname, identity field and matching passwords; the CN source says “valid data” without those literal values.
- **Expected (CN):** a registration request is created and visible in administration.
- **Actual (CN):** the message **“Usuario creado correctamente”** appeared, and the request was visible in administration.
- **Original CN status:** `Aprobado ID`.
- **Source discrepancy:** the CP's final “Resultado obtenido” uses a generic statement; its “Resultado real (caja negra)” and the CN record contain the more specific observation.

### CP-074 / CN-002 — Registration with all fields empty

- **Dataset:** `JD-074` (marked `Diseñado`).
- **Precondition:** the registration form is open.
- **Input (CN):** *all* form fields empty.
- **Expected (CN):** registration is blocked and required fields are identified.
- **Actual (CN):** **“El correo es obligatorio.”** appeared and no registration request was created.
- **Original CN status:** `Aprobado ID`.
- **Source discrepancy:** JD-074 also shows a *valid existing identity* and an *Inspector role*, inconsistent with the all-empty scenario. Those JD fields are not treated as the verified inputs of CN-002.

### CP-075 / CN-003 — Registration with invalid email

- **Dataset:** `JD-075` (marked `Diseñado`).
- **Precondition:** registration interface available.
- **Input (CN):** `pruebacorreo.com` (missing `@`), other mandatory fields valid but unspecified.
- **Expected (CN):** reject the email, show validation and create no request.
- **Actual (CN):** **“Ingresá un correo electrónico válido.”** appeared and no request was generated in administration.
- **Original CN status:** `Aprobado ID`.
- **Source limitation:** JD-075 includes a generic existing identity/role context that is not established by the CN narrative.

### CP-094 / CN-022 — Inspector tries to open the Administrator interface

- **Dataset:** `JD-094` (marked `Diseñado`).
- **Precondition:** an enabled Inspector is signed in.
- **Input:** direct navigation to an Administrator-only URL. The literal URL is not specified in the black-box case.
- **Expected (CN):** access is denied by redirecting to an allowed page or displaying a denial.
- **Actual (CN):** redirect to the login page **without a message**; Administrator-only content was not visible.
- **Original CN status:** `Aprobado Identificador`.
- **Interpretation limit:** the source does not establish whether the Inspector session remained valid after the redirect.

### CP-118 / CN-046 — Municipal Inspector supervises route creation (**Fail**)

| Field | Original record |
| --- | --- |
| Related use case | `CU_16 — Inspector municipal supervisa la creación de rutas` |
| Dataset | `JD-118`, marked `Diseñado` |
| Preconditions | Inspector authenticated, application available, existing routes and vehicles, supervision section accessible |
| Input / preparation | Inspector session; route-supervision interface; existing route and vehicle records; JD-118 also includes consultation date `2026-11-05` |
| Expected (CN) | Inspector can view assignments, access **“Registrar ruta de recolección”**, and access **“Asignar camión a ruta”** |
| Actual (CN) | Existing assignments were visible, but both the route-registration and truck-assignment options were unavailable |
| Original status | **`Fail`** |
| Follow-up | Fix and retest **not confirmed**; do not mark as resolved or currently reproducible |

**Traceability note:** the discrepancy is relevant to `RF-22` (assign vehicles to collection routes) and `RF-23` (register collection routes). The integrator's CP-118 has a generic final “Resultado obtenido”; the more precise failed observation and status come from the independent CN-046 record.

## 4. What these records demonstrate—and what they do not

**Supported by the supplied documents:** structured test case documentation, valid/invalid input selection, expected-versus-actual comparison, use-case traceability, role-based access scenarios, and a real recorded mismatch (`CN-046`).

**Not independently established:** which teammate personally executed each case, the precise live test database contents, timestamps/logs for each request, measured statement/branch coverage, automated test runs, or whether the recorded failure was later fixed. Most `JD-073`–`JD-125` entries carry a `Diseñado` label, so a JD reference must not be presented as a separate proven execution.

## 5. Document quality observations

1. **Generic results:** a number of CP records use broad “the system responds accordingly” statements even when their associated CN cases describe actual UI messages and outcomes.
2. **Conflicting inputs:** `JD-074` lists nonempty contextual values despite an all-fields-empty scenario; `CP-079` also requires reconciling a confirmation-password-empty scenario with tabulated values before reuse.
3. **Reproducibility gaps:** inputs such as “existing record in test DB” are conditions, not complete datasets that another tester can recreate unaided.
4. **Unresolved defect history:** `CN-046` is recorded as Fail, with no confirmed corrective action or retest.
5. **Status vocabulary:** `Aprobado ID`, `Aprobado Identificador`, `Pass`, and `Fail` appear in the original black-box documents; statuses have not been rewritten in this file.

## 6. Source references

- *DataByte — Tercer Avance de Proyecto*, v3.0, **section 3.5.4**, printed pp. 210–326. In particular: `CP-001`–`CP-003` (pp. 210–212), `CP-073`–`CP-075` (pp. 263–266), `CP-094` (pp. 287–288), and `CP-118` (pp. 317–318).
- *Caja Negra DataByte*, **CN-001, CN-002, CN-003, CN-022, CN-046**, corresponding individual test records.

**Safety:** This file omits passwords, administrator secrets, private credentials, real team identity details, and internal infrastructure addresses. Placeholder test data is presented only as described by the academic source. No original source PDF or API environment is published here.
