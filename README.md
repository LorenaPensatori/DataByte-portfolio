# DataByte — Software QA Portfolio

**Academic software testing project | Uruguay | 2026**

This repository documents quality assurance activities carried out as part of **DataByte**, a collaborative academic software project developed by a three-member team at **Escuela Superior de Informática (UTU), Uruguay**.

I am **Lorena Pensatori**, an Information Technology graduate interested in beginning my career as a **Junior QA Analyst**, particularly in the video game industry. I participated in DataByte as a project coordinator and development team member, collaborating on software development, functional validation, and technical documentation.

> **Scope and attribution:** Testing artifacts in this repository represent work produced by the **DataByte team**. They are not presented as tests individually authored or executed by me. Results are historical records from our academic documentation, not new test executions or an independent certification of the system.

## Project overview

DataByte developed **SiGeRU (Sistema de Gestión de Residuos Urbanos)**, a web-based system intended to support urban waste management. Its documented features include user registration and role-based access, container and incident management, collection routes, crews, vehicles, and facilities.

The documented solution uses web interfaces, REST APIs, and a relational database. Its technical documentation describes **PHP 8, MySQL 8, HTML, CSS, JavaScript, MVC architecture, Git/GitHub, Figma, and Postman**. The testing plan also references a **MySQL/MariaDB** local testing environment.

## QA work documented in the project

| Area | Evidence in the academic documentation |
| --- | --- |
| Test planning | Scope, risks, entry/exit criteria, test environment, and responsibilities |
| Black-box testing | 53 functional test cases (CN-001 to CN-053) with expected and recorded actual results |
| Test cases and test data | 125 cases (CP-001 to CP-125) and corresponding test-data identifiers (JD-001 to JD-125) |
| White-box testing | Control-flow analysis and basic-path test design for five PHP `Usuario` methods |
| API testing | 11 documented POST creation scenarios tested using Postman, with HTTP 201 responses recorded |
| Defect identification | CN-046 records a **Fail**: the Municipal Inspector could view routes but could not access the documented route-creation or vehicle-assignment actions |

### Example: a documented failed test

**CN-046 — Municipal Inspector: route creation and supervision**

- **Expected:** The Municipal Inspector can access route registration and truck assignment functionality in addition to viewing route information.
- **Recorded actual result:** Viewing existing routes and assignments was available, but the options to register routes and assign trucks were not available.
- **Recorded status:** **Fail**.
- **Follow-up:** Whether this issue was fixed or retested is **not confirmed** in the available records.

This example illustrates the difference between an **expected result** and an **actual result**, and why a failure should remain visible in a testing record.

## What this portfolio demonstrates

The documentation provides examples of understanding requirements, preparing valid and invalid test inputs, describing test steps, comparing expected and actual behavior, analyzing code paths, and documenting API responses.

**Evidence limitations:** The historical documents do not independently verify every execution, individual tester attribution, automated test coverage, regression results, or the resolution of CN-046. The white-box coverage figures reported in the academic document should not be interpreted as independently measured coverage-tool output. This repository does not claim professional game-testing experience or automated testing expertise.

## Portfolio contents

This repository contains selected QA documentation from the collaborative DataByte academic project:

- [System Overview](documentation/system-overview.md): project context, intended functionality, architecture and QA scope.
- [Black-box Testing](testing/black-box-testing.md): selected functional scenarios and their historically recorded outcomes.
- [White-box Testing](testing/white-box-testing.md): control-flow analysis, cyclomatic complexity and basis-path test design.
- [Test Cases](testing/test-cases.md): 125 documented test cases and their traceability references.
- [Test Data](testing/test-data.md): 125 associated test-data records and documented preconditions.
- [API Testing](testing/api-testing.md): 11 documented POST creation scenarios with recorded HTTP 201 responses.

## Responsible sharing

This repository contains curated summaries of collaborative academic work. Original source documents, credentials, personal identifiers, internal infrastructure details and other sensitive information are excluded.
Historical results are presented with their original evidence limitations. No new test executions, fixes or independent coverage measurements are implied. Original academic PDFs are **not** included here.

## About me

**Lorena Pensatori**  
Information Technology graduate — Escuela Superior de Informática (UTU), Uruguay  
Career goal: **Junior QA Analyst**  
English: Intermediate (conversational)  
Availability: Full-time

I am particularly interested in applying careful observation, reproducible test design, and clear defect communication to software quality assurance in games.
