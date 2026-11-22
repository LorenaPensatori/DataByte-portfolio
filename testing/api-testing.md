# API Testing with Postman — DataByte (SiGeRU)

**Academic software quality assurance | Collaborative DataByte team project | 2026**

## Scope and source

DataByte's *Documento Integrador — Tercera Entrega*, section **3.3.13, “Pruebas de API y Validación de Endpoints (Postman)”**, printed pages **139–150** (PDF viewer pages 140–151), records **11 backend creation tests** made with Postman. The report shows JSON request payloads, expected HTTP responses, stated actual results and embedded Postman screenshots.

The tests focus on the interaction with the PHP backend through HTTP endpoints and the creation of entities for the urban waste management system. They are historical **team-project evidence**, not tests newly executed for this portfolio.

**Documented result:** all 11 tests state **“Exitoso. Código 201 Created”**. This is a reported status-code result, **not proof of comprehensive database persistence, referential integrity, or API security checks**.

### Test inventory

| Portfolio ref | Original report case | Operation | HTTP method | Expected | Reported actual |
| --- | --- | --- | --- | --- | --- |
| `API-01` | Case 1 | Create user | `POST` | `201 Created` | `201 Created` |
| `API-02` | Case 2 | Create container | `POST` | `201 Created` | `201 Created` |
| `API-03` | Case 3 | Create incident | `POST` | `201 Created` | `201 Created` |
| `API-04` | Case 4 | Create municipality | `POST` | `201 Created` | `201 Created` |
| `API-05` | Case 5 | Create establishment | `POST` | `201 Created` | `201 Created` |
| `API-06` | Case 6 | Create vehicle | `POST` | `201 Created` | `201 Created` |
| `API-07` | Case 7 | Create service / maintenance | `POST` | `201 Created` | `201 Created` |
| `API-08` | Case 8 | Create machinery | `POST` | `201 Created` | `201 Created` |
| `API-09` | Case 9 | Create crew | `POST` | `201 Created` | `201 Created` |
| `API-10` | Case 10 | Create route | `POST` | `201 Created` | `201 Created` |
| `API-11` | Case 11 | Create unloading | `POST` | `201 Created` | `201 Created` |

The `API-##` labels are **portfolio navigation IDs**; the integrator labels the records “Caso de Prueba 1” through “Caso de Prueba 11”. They should not be confused with `CP-###` identifiers from the separate test-case inventory.

## Original request examples (sanitized)

Each endpoint below is shown with its **source path** and an example JSON body transcribed from the integrator. The original environment used `http://localhost/DataByte/` as its base URL; these are **historical local examples, not active public API addresses**. JSON field names and source values are preserved except the sample registration password, which is replaced with a visible redaction marker. **The redacted JSON must not be mistaken for a directly executable, verified fixture.**

### API-01 — Create User

- **Original test:** Case 1
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Usuarios/public/index.php`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "correo": "juan@mail.com",
  "CI": 1234567,
  "contrasena": "[REDACTED_TEST_PASSWORD]",
  "conf_contrasena": "[REDACTED_TEST_PASSWORD]",
  "PrNom": "Juan",
  "PrApel": "Pérez",
  "rol": "usuario",
  "estado_habil": "pendiente"
}
```

**Source observation:** Registration endpoint; the original report includes an example password that is intentionally not reproduced.

### API-02 — Create Container

- **Original test:** Case 2
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Gestion/public/index.php?accion=contenedores`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_contdor": 5,
  "estado_optivo": "funcional",
  "en_uso": "desplegado",
  "tipo": "rotura",
  "calle": "Calle 8",
  "nmro": 15,
  "esq": "Av. Libertad",
  "codigo": "B",
  "matricula": "125"
}
```

**Source observation:** The source uses field names exactly as shown here.

### API-03 — Create Incident

- **Original test:** Case 3
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Recoleccion/public/index.php?accion=incidencias`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_incidencia": 6,
  "id_contdor": 5,
  "tipo": "desborde",
  "estado": "abierta",
  "fch_apert": "2025-09-13",
  "fch_resol": "",
  "nom_cuadrilla": "Cuadrilla Norte"
}
```

**Source observation:** The expected result additionally states that the incident should be registered and linked correctly; the reported actual explicitly lists only HTTP 201.

### API-04 — Create Municipality

- **Original test:** Case 4
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Gestion/public/index.php?accion=municipios`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "codigo": "D"
}
```

**Source observation:** The expected result additionally states the municipality should be registered and linked correctly; the reported actual explicitly lists only HTTP 201.

### API-05 — Create Establishment

- **Original test:** Case 5
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Gestion/public/index.php?accion=establecimientos`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_establcmto": 3,
  "nombre": "Centro 1",
  "calle": "Av. principal",
  "nmro": 125,
  "esq": "Calle 2",
  "tipo": "centro de acopio",
  "capac_actual": 50.0,
  "capac_max": 100.0,
  "tipo_res": "orgánica"
}
```

**Source observation:** The expected result refers to successful registration and linkage; the recorded actual lists HTTP 201.

### API-06 — Create Vehicle

- **Original test:** Case 6
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Recoleccion/public/index.php?accion=vehiculos`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "matricula": "ABC1234",
  "estado_optivo": "funcional"
}
```

**Source observation:** The expected result refers to successful registration and linkage; the recorded actual lists HTTP 201.

### API-07 — Create Service / maintenance

- **Original test:** Case 7
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Gestion/public/index.php?accion=mantenimientos`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_serv": 1,
  "estado": "en_curso",
  "fecha": "2025-09-13",
  "tipo": "preventivo"
}
```

**Source observation:** The expected result refers to successful registration and linkage; the recorded actual lists HTTP 201.

### API-08 — Create Machinery

- **Original test:** Case 8
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Gestion/public/index.php?accion=maquinarias`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_maquinaria": 1,
  "nombre": "Excavadora 01",
  "en_uso": "desplegado",
  "id_establcmto": 1,
  "id_serv": 1
}
```

**Source observation:** The payload references establishment ID 1, whereas API-05 documents establishment ID 3; pre-existing data may have existed, but the report does not establish the setup. No failure can be inferred.

### API-09 — Create Crew

- **Original test:** Case 9
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Recoleccion/public/index.php?accion=cuadrillas`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "nom_cuadrilla": "Cuadrilla Norte",
  "cedula_inspector": 12
}
```

**Source observation:** The payload references an inspector identity but the test does not document how that record was prepared.

### API-10 — Create Route

- **Original test:** Case 10
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Recoleccion/public/index.php?accion=rutas`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "id_ruta": 5,
  "nom": "Ruta Centro",
  "matricula": "ABC1234"
}
```

**Source observation:** The API recorded 201; this does not resolve the separate CN-046 interface-level failure to access route creation and truck assignment as Municipal Inspector.

### API-11 — Create Unloading

- **Original test:** Case 11
- **Method:** `POST`
- **Endpoint:** `http://localhost/DataByte/API_Recoleccion/public/index.php?accion=descargas`
- **Expected status:** `201 Created`
- **Recorded actual:** `Exitoso. Código 201 Created`

```json
{
  "matricula": "ABC1234",
  "id_establcmto": 1,
  "hora": "08:30:00",
  "peso": 1200.5
}
```

**Source observation:** The payload references establishment ID 1; existence is not independently verified by the written API result.

## Cross-layer testing insight: API-10 versus CN-046

The integrator records **API Case 10** (create route) as a successful `POST` with status `201 Created`. Separately, the independent black-box report records **`CN-046` as `Fail`**: a Municipal Inspector could inspect route information but could not access the UI options to **register a route** or **assign a truck**.

These statements describe **different testing layers and access paths**. Successful direct API creation does not demonstrate that the intended role can reach the operation through the application interface. The available records do **not** establish whether the API route had authorization vulnerabilities, why the UI options were absent, or whether the issue was later fixed. No root cause is asserted.

## Evidence strength and limitations

**What the academic documentation supports**

- 11 named `POST` creation scenarios with their request payloads and stated expected/actual HTTP status codes.
- Embedded Postman screenshots in the original integrator, adjacent to the cases.
- Use of local endpoints grouped under `API_Usuarios`, `API_Gestion`, and `API_Recoleccion`.
- A direct comparison between an API-level success case and an independent interface-level failure on route functionality.

**What the documentation does not independently establish**

- Full response bodies, schema assertions, or automated test scripts for every case.
- Confirmation of database row creation, referential integrity or transactional behavior for every request (even where the expected outcome mentions linkage).
- A recorded battery of negative API tests (`400`, `401`, `403`, `404`), `PUT`, `PATCH`, or `DELETE` requests in this section.
- Repeatable test-environment setup and exact prerequisite records for the foreign keys used by all payloads.
- Collection-run results, performance metrics, CI/CD integration, or coverage measurement.

### Suggestions for future testing (not historical evidence)

If the system is available again, a new test run could include negative payload validation, unauthorized and forbidden requests, duplicate entities, foreign-key failures, assertions on response JSON, and database-state verification. These are **proposed follow-ups only** and must not be represented as completed tests.

## Privacy, security, and provenance

This portfolio document does not include actual passwords, administrative server secrets, private identities, internal IP addresses, or the original PDFs. The original local `localhost` paths are retained to explain endpoint organization; they are not deployment instructions. Source test password strings are intentionally redacted in the displayed payloads.

**Primary source:** *Documento Integrador — DataByte, Tercera Entrega*, §3.3.13, printed pp. 139–150. **Related source:** *Caja Negra DataByte*, case CN-046. All source material belongs to the collaborative DataByte academic project.
