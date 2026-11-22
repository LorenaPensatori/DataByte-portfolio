# Black-Box Testing — DataByte (SiGeRU)

**Academic QA evidence | DataByte team | Uruguay, 2026**

> **Evidence and attribution.** This document is a curated English-language presentation of cases recorded in the team's original 118-page *Caja Negra DataByte* document (dated 5 November 2026). The original records are collective project evidence; no individual authorship or execution is claimed here. These are **historical recorded results**, not newly executed tests or independently verified system behavior. Original PDFs are not republished.

## 1. Purpose and approach

Black-box testing evaluates **observable software behavior** against stated requirements without needing to inspect the implementation. DataByte's original records label the work as *Caja negra — prueba funcional* or, in some cases, *Caja negra — partición de equivalencia / validación funcional*. They describe system-level tests with preconditions, input data, steps, expected behavior, recorded results, and status.

The documented functional areas include registration, authentication, role-based access, administration, and workflows for urban waste management. This selection deliberately includes valid inputs, invalid inputs, unauthorized access, and a failed functional scenario.

**ID conventions:** `CN-###` identifies a black-box case in the independent source; corresponding `CP-###` and `JD-###` entries appear in section 3.5.4 of the DataByte integrator. These links represent **documentary traceability**, not additional executions.

## 2. Selected original scenarios

### CN-001 — User registration with valid data

| Field | Recorded information |
| --- | --- |
| Test type / level | Functional / System |
| Priority | High |
| Related area | User registration |
| Preconditions | User has not previously registered |
| Test inputs | Valid email, first name, surname, identity-number field, and matching passwords; this black-box record does not specify their literal values |
| Procedure | Submit the registration form with valid data; check the operation result |
| Expected result | Registration request is created and becomes visible in administration |
| Actual result recorded | Message **“Usuario creado correctamente”** appeared and the request became visible in the administration interface |
| Original status | `Aprobado ID` |
| Integrator reference | `CP-073` / `JD-073` |

### CN-002 — Registration with all fields empty

| Field | Recorded information |
| --- | --- |
| Test type / technique | Functional; equivalence partitioning / functional validation |
| Priority | High |
| Preconditions | Registration form is open |
| Test inputs | All form fields left empty |
| Procedure | Click **“Crear cuenta”** without completing any fields; check the result |
| Expected result | Registration is blocked and missing required fields are indicated |
| Actual result recorded | Message **“El correo es obligatorio.”** appeared; no registration request was created |
| Original status | `Aprobado ID` |
| Integrator reference | `CP-074` / `JD-074` |

### CN-003 — Registration with an invalid email format

| Field | Recorded information |
| --- | --- |
| Test type / technique | Functional; equivalence partitioning / functional validation |
| Priority | High |
| Preconditions | Registration interface is open |
| Test inputs | Email `pruebacorreo.com` (missing `@`); other required fields valid, without literal values supplied in the CN record |
| Procedure | Complete the form, enter the invalid email, click **“Crear cuenta”**, and check the result |
| Expected result | Invalid email is rejected with a validation message; no registration request is created |
| Actual result recorded | Message **“Ingresá un correo electrónico válido.”** appeared; no request was created in administration |
| Original status | `Aprobado ID` |
| Integrator reference | `CP-075` / `JD-075` |

### CN-022 — Inspector attempts to access the Administrator interface

| Field | Recorded information |
| --- | --- |
| Test type / level | Functional / System |
| Priority | High |
| Related area | Role-based access control |
| Preconditions | An Inspector account is enabled and has an active authenticated session |
| Test inputs | Inspector session; direct URL to the Administrator interface (the literal URL is not provided in the CN record) |
| Procedure | Sign in as Inspector; enter the Administrator interface URL directly; observe the response |
| Expected result | Administrator-only content is inaccessible; redirect to an allowed page or display an access-denied message |
| Actual result recorded | User was redirected to the login page **without a message**; Administrator-only content was not displayed |
| Original status | `Aprobado Identificador` |
| Integrator reference | `CP-094` / `JD-094` |

**Interpretation limit:** This checks the observed restricted-page behavior. The source does not establish whether the session should persist after redirect, nor does it demonstrate a penetration test.

### CN-046 — Municipal Inspector supervises route creation (**Fail**)

| Field | Recorded information |
| --- | --- |
| Test type / level | Functional / System; source technique marked *Caso de uso* |
| Priority | High |
| Related use case | `CU_16 — Inspector municipal supervisa la creación de rutas` |
| Preconditions | Inspector authenticated; system available; routes and vehicles registered; supervision section accessible |
| Test inputs | Inspector session, route-supervision section, existing routes and vehicles |
| Step 1 — supervision page | **Expected:** access allowed. **Actual:** access successful |
| Step 2 — view routes/vehicles | **Expected:** assigned routes and vehicles visible. **Actual:** displayed correctly |
| Step 3 — route registration | **Expected:** **“Registrar ruta de recolección”** option available. **Actual:** option unavailable |
| Step 4 — truck assignment | **Expected:** **“Asignar camión a ruta”** option available. **Actual:** option unavailable |
| Step 5 — use those options | **Expected:** the selected function can be accessed. **Actual:** neither function could be accessed |
| Original status | **`Fail`** |
| Integrator reference | `CP-118` / `JD-118` |

**Recorded discrepancy:** The Inspector could consult existing routes and vehicle assignments but could not register routes or assign trucks, despite those actions being expected in the use case. The integrator's functional requirements `RF-22` (assign trucks to routes) and `RF-23` (register routes) also describe these capabilities for the Inspector role.

**Follow-up status:** No confirmed fix, retest, root cause, or current reproducibility is available. The historical status remains **Fail**. The test document does not include an independently verifiable defect-tracking ticket.

## 3. Evidence assessment

| Evidence category | What the sources support | Limitation |
| --- | --- | --- |
| Test design | Cases include objectives, conditions, steps and expected outcomes | Some input values are only described generically |
| Test execution records | The PDF states actual results and pass/fail-type statuses | No separate screenshots or execution logs were retained for these CN cases |
| Negative testing | Required fields, invalid formats, and unauthorized access were tested | No claim of exhaustive boundary/security coverage |
| Defect identification | CN-046 explicitly documents a failed expected behavior | No verified fix or retest |
| Traceability | Original CN IDs correspond to CP/JD entries in the integrator | Some CP/JD field values and summary wording do not fully reconcile with the original CN descriptions |

### Notes on statuses and reproduction

The source uses several labels, including `Aprobado ID`, `Aprobado Identificador`, `Pass`, and `Fail`; this document preserves each selected case's original label rather than silently standardizing it. The original CN files do not provide a universal test environment/build identifier or a full reproducible input dataset for every scenario. Test results should therefore be understood as **documented historical observations**.

## 4. Connection to QA practice

These cases illustrate how a QA analyst can:

1. Derive an observable expected behavior from a requirement or use case.
2. Select positive and negative inputs (including equivalence classes, where explicitly identified in the source).
3. Compare a recorded actual result with the expected outcome.
4. Preserve a failed result and distinguish it from any unverified fix.
5. Maintain links among requirements, test cases, and test data.

They do **not** establish personal ownership of every case, professional game-testing experience, automated test execution, or independent certification of the results.
