# TINF24F_1-UTITP-0v2 — Test Plan for Unit & Integration Tests (UT/IT)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)  
**Team:** Team 1  
**Author:** Priyanshu (Software Tester / Test Manager)  
**Date:** 2026-04-29
**Status:** v0.2 — updated with existing automated test coverage as supporting evidence

---

## Version Control

| Version | Date | Author | Notes |
|---|---|---|---|
| 0.1 | 2026-02-14 | Priyanshu | Initial UT/IT planning document |
| 0.2 | 2026-04-29 | Priyanshu | Updated after branch integration; maps existing automated tests to tester evidence without claiming implementation ownership |

---

## 1. Purpose

This document supports the Unit/Integration Test planning requirement. It is not a replacement for the STP/STR. The lecturer-facing system test evidence remains documented in the STP and STR.

Priyanshu's role boundary:

- **Implemented/documented by Priyanshu:** AASX CD Importer and its MOD.
- **Managed/tested by Priyanshu:** system test planning, traceability, execution evidence, defect reporting.
- **IEC Importer:** implemented by another team member; Priyanshu uses available automated tests and manual black-box tests as Test Manager evidence only.

---

## 2. Test Levels

| Level | Purpose | Backend needed | Evidence use |
|---|---|---|---|
| Unit tests | Test functions, utilities, validators, and composables in isolation | No | Supports regression confidence |
| Integration tests | Test cooperation between composables, clients, routing, and data transformation logic | Usually mocked; sometimes local backend | Supports module integration confidence |
| System tests | Test complete user workflows through the Web UI | Yes | Main lecturer deliverable in STP/STR |

---

## 3. Existing Automated Tests in the Web UI Repo

The project uses **Vitest** and follows the existing `*.test.ts` convention under `aas-web-ui/tests/`.

Relevant current test areas:

| Area | Example test files | Tester interpretation |
|---|---|---|
| AASX import | `tests/composables/AAS/AASXCdImport.test.ts`, `AASXImport.test.ts`, `AASXPackaging.test.ts` | Supports Priyanshu's owned AASX importer module |
| IEC import validation | `tests/composables/IecCddValidator.test.ts`, `IecFileImport.test.ts` | Supports integrated IEC Importer testing; implementation ownership remains with teammate |
| Repository/request behavior | `tests/composables/RequestHandling.test.ts`, `SMRepositoryClient.test.ts`, `DescriptorSync.test.ts` | Supports backend/client reliability checks |
| Routing/module integration | `tests/router/moduleRouteManifest.test.ts` | Supports module registration/navigation confidence |
| Semantic and AAS utilities | `tests/utils/AAS/SemanticIdUtils.test.ts`, `DescriptorUtils.test.ts`, `SubmodelElementPathUtils.test.ts` | Supports AAS/IEC semantic ID behavior |
| UI/component utilities | `tests/components/SubmodelElements/*.test.ts` | Supports existing UI behavior around submodel elements |

These tests are used as supporting evidence for quality assurance. This document does not claim Priyanshu authored every existing automated test.

---

## 4. UT/IT Scope for the Final Submission

In scope:

- Run the existing automated test suite before final STR update.
- Record command results in the STR.
- Use automated tests as supporting evidence for AASX import, IEC data validation, routing, and repository/client behavior.
- Document failures as defects or risks instead of silently changing application code.

Out of scope for Priyanshu:

- Implementing IEC Importer code.
- Writing IEC module documentation.
- Fixing application defects without separate approval.
- Replacing system tests with unit tests. The professor's minimum strategy remains requirements-based system testing.

---

## 5. Commands

Run from:

```bash
cd Team1-basyx-aas-web-ui/aas-web-ui
```

Final evidence commands:

```bash
npm run lint:check
npm run test:run
npm run type-check
npm run build
```

The result of each command is recorded in the STR with date, branch, and commit hash.

---

## 6. Evidence Mapping to STP/STR

| STP/STR area | Automated support | Manual system test still required |
|---|---|---|
| CD-Manager core navigation/table/detail | Routing and shared component tests | Yes: Web UI workflow must be executed manually |
| AASX CD Importer | AASX import/composable tests | Yes: scan/import/re-import/invalid file workflow |
| IEC Importer | IEC validator/file import tests | Yes: upload valid/invalid file and save to repository |
| Repository connectivity | Request/client-related tests | Yes: real cd-repo URL configured/missing behavior |
| NFR checks | Lint/type-check/build support maintainability | Yes: usability, responsive layout, console errors |

---

## 7. Priority Before Final Presentation

1. Execute automated checks and save the summaries for STR evidence.
2. Execute the required manual system tests from STP v0.3.
3. Update STR v0.3 with pass/fail/blocked results and evidence references.
4. Use only the final STR counts in the presentation.

---

## 8. Notes

- Automated tests improve confidence but do not replace the lecturer-required requirements-based system test.
- Failed automated checks are reported in the STR as quality risks.
- Any defect must reference the failing command or STP test case ID.
