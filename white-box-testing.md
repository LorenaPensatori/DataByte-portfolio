# White-Box Testing — DataByte

**Project:** DataByte / SiGeRU (2026)  
**Context:** Collaborative academic software project (three-member team)  
**Source:** *DataByte — Third Project Delivery*, version 3.0 (6 November 2026), section **3.5.2, “Caja blanca”**, printed pp. **168–208** (PDF pages 169–209).

> **Evidence note:** This portfolio page summarizes the team's original white-box testing *documentation*. The source contains PHP code excerpts, control-flow graphs, cyclomatic-complexity calculations, basic paths, test-case specifications, and coverage tables. It does **not** provide independently verifiable execution logs or automated coverage reports for every path. Reported coverage percentages below are therefore **documented claims**, not independently measured coverage. No tests were rerun to prepare this page.

## Purpose and approach

White-box testing examines a program's internal structure, including branches, decisions, error exits, and database operations. The DataByte team documented **basis-path testing** for five methods of the PHP `Usuario` class. For each method, the document presents a control-flow graph, calculates **cyclomatic complexity** using `V(G) = E − N + 2` and `V(G) = P + 1`, and maps independent paths to test specifications.

The case IDs `CP-01`, `CP-02`, etc. are **local to each method's white-box analysis**; they are **not** the same as the integrator's functional cases `CP-001` through `CP-125`.

## Inventory of analyzed methods

| PHP method | Graph nodes (N) | Graph edges (E) | Decisions (P) | Documented V(G) | Basic paths documented | Printed pages |
|---|---:|---:|---:|---:|---:|---|
| `Usuario::create()` | 26 | 38 | 13 | 14 | 14 | 168–175 |
| `Usuario::login()` | 12 | 16 | 5 | 6 | 6 | 176–182 |
| `Usuario::asignarRol()` | 51 | 74 | 24 | 25 | 24 executable paths; one path classified as infeasible | 182–193 |
| `Usuario::delete()` | 16 | 22 | 7 | 8 | 8 | 194–201 |
| `Usuario::update()` | 15 | 20 | 6 | 7 | 7 | 202–208 |

**Counting convention:** The source explicitly treats the PHP null-coalescing operator (`??`) as an expression rather than a separate decision when calculating complexity for `create()` and `update()`. Results above retain that convention.

## Representative test designs from the original documentation

The following are **expected results in the white-box test specifications**, not newly observed execution results. The method-level `CP` identifiers have been preserved.

### 1. `Usuario::create()` — registration and database handling

The method normalizes roles, checks administrator references, hashes a password, and attempts to insert a user.

| Case | Basic path | Input / setup summarized | Expected result in source |
|---|---|---|---|
| `CP-01` | `C1` | Valid `USUARIO` role and existing administrator reference | Returns `true`; user inserted with role `USUARIO` and pending status |
| `CP-02` | `C2` | Unsupported role `SUPERUSER` | Returns `true`; role normalized to `USUARIO` |
| `CP-03` | `C3` | `ADMIN` role without an administrator reference | Returns `false`; nothing inserted |
| `CP-14` | `C14` | User insertion fails (for example, duplicate ID) | Returns `false` |

The original analysis also specifies failure-injection scenarios in which `mysqli::prepare()` or `execute()` fails. **Some require mocks**; their inclusion in the design does not prove that mocks or a specific test framework were actually used.

*Source: section 3.5.2, printed pp. 168–175.*

### 2. `Usuario::login()` — authentication decisions

| Case | Basic path | Scenario | Expected result in source |
|---|---|---|---|
| `CP-01` | `C1` | Existing approved account, correct password | Successful login; user details returned without password |
| `CP-03` | `C3` | Account not found | Failure with “Usuario no encontrado” |
| `CP-04` | `C4` | Incorrect password | Failure with “Contraseña incorrecta” |
| `CP-05` | `C5` | Correct password, account pending | Access denied pending approval |
| `CP-06` | `C6` | Correct password, account rejected | Access denied due to rejection |

`CP-02` covers a failed `prepare()` operation. The analysis emphasizes the order of validation: user existence, password, then approval status.

*Source: printed pp. 176–182.*

### 3. `Usuario::asignarRol()` — role-specific conditions and transactions

The original graph has **V(G) = 25**. The document explicitly identifies **one infeasible path** and derives **24 executable basic paths** instead of presenting 25 executable cases. Scenarios address role-specific requirements for `INSPECTOR`, `PEON`, `CHOFER`, and `OPERARIO_ESTABLECIMIENTO`, including related-record checks and rollback after failures.

The documentation describes tests of transactional **atomicity**: when an update succeeds but a subsequent role-related validation or insert fails, the database transaction should be rolled back. This is described in the source's design and justification; independent execution artifacts are not provided here.

*Source: printed pp. 182–193.*

### 4. `Usuario::delete()` — deletion and rollback

The analysis includes **eight basic paths**, plus **two supplementary cases** that explore behavior not represented by a new basic path. It covers failures at transaction start, statement preparation, execution, and commit, as well as successful deletion of user and role-detail records.

The document highlights coverage of the first and last iterations of the deletion loop and a complete traversal. Because deletion is destructive, verifying that unrelated records remain unchanged is an important consideration.

*Source: printed pp. 194–201.*

### 5. `Usuario::update()` — partial updates

| Case | Basic path | Scenario | Expected result in source |
|---|---|---|---|
| `CP-01` | `C1` | Valid role, status, and new password | Returns `true`; values and hashed password updated |
| `CP-02` | `C2` | Unsupported role | Role replaced by `USUARIO` |
| `CP-03` | `C3` | Empty `estado_habil` | Status saved as `pendiente` |
| `CP-04` | `C4` | No new password | Other values updated; previous password retained |
| `CP-07` | `C7` | Update execution fails | Returns `false`; row unchanged |

`CP-05` and `CP-06` describe `prepare()` failure scenarios and require an injected failing database double. The source mentions PHPUnit's `createMock()` **as a possible implementation**, not as proof that PHPUnit was used.

*Source: printed pp. 202–208.*

## Reported structural coverage

For each of the five methods, the project's original documentation presents coverage tables reporting **100% of the graph nodes, edges, decision branches, and considered basic paths**. For `asignarRol()`, the infeasible path is excluded from the set of 24 executable paths.

**Interpretation:** These are **coverage results reported by the academic analysis**, under its graph model and path-counting assumptions. Without an execution report or instrumentation output, this portfolio does **not** claim independently verified 100% runtime coverage. Basis-path coverage should also not be confused with exhaustive testing of every possible input or every complete execution path.

## Findings identified in the source analysis

These are **observations documented in the white-box analysis**, not new bugs discovered while creating this portfolio:

1. **`delete()` and nonexistent users:** A database delete may complete without an SQL error even if no user row matches, potentially allowing the method to return `true` without deleting a user. *Source: printed pp. 200–201.*
2. **`update()` and nonexistent users:** A successful `UPDATE` execution may return `true` even if no existing user row was updated; the method does not first verify existence. *Source: printed p. 208.*
3. **`update()` and unintended defaults:** When role or approval status is omitted, the method can assign defaults (`USUARIO`, `pendiente`) and overwrite previously stored values. *Source: printed p. 208.*
4. **Statement cleanup:** The analysis notes that a prepared statement is not closed on certain failed `execute()` branches in `update()` and `create()`. *Source: printed p. 208.*
5. **Security review item:** The original `create()` code excerpt contains a hard-coded default administrator password. Its value is intentionally **not reproduced** in this public-facing summary. This is a risk to assess, not evidence of exploitation or proof that the credential is currently active. *Source: printed p. 169.*

**Resolution status:** The reviewed documentation does not establish whether these observations were converted into defect tickets, fixed, or retested. No resolution is claimed.

## Evidence boundaries and reproducibility

- **Available:** Original source excerpts, flow graphs, path descriptions, test inputs and expected outputs, and documented coverage tables.
- **Not established:** Complete individual execution logs, current test environment, executable test suite, PHPUnit usage, independent coverage instrumentation, fixes, or retest evidence.
- **Team attribution:** All documented testing artifacts are treated as **collaborative DataByte work**, rather than assigned to a particular student.
- **Safe publication:** Original PDFs, credentials, real identifiers, live internal URLs, and infrastructure secrets should not be uploaded without explicit permission and a separate sanitization review.

## What this work demonstrates

This documentation provides evidence of exposure to **control-flow analysis, cyclomatic complexity, decision-based test design, database failure scenarios, and analysis of internal software behavior**. It complements the black-box tests, which evaluate visible behavior without using implementation details.

**Original reference:** *DataByte — Tercer Avance de Proyecto*, version 3.0, section 3.5.2, “Caja blanca,” printed pages 168–208 (academic documentation, 2026).
