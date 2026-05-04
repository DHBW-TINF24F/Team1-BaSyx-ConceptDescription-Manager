# TINF24F_1-STR-0v3 - System Test Report (STR)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)  
**Team:** Team 1  
**Role owner:** Priyanshu (Test Manager)  
**Date:** 2026-05-02  
**Status:** v0.3 - Browser system-test evidence captured; importer tests passed; CD-Manager core view unavailable

---

## Version Control

| Version | Date | Author | Notes |
|---|---|---|---|
| 0.1 | 2026-03-31 | Priyanshu | Initial execution - AASX CD Importer tests completed on the earlier AASX branch context |
| 0.2 | 2026-04-29 | Priyanshu | Updated after branch integration; removed outdated branch-blocker wording; added ownership boundary, IEC black-box test scope, and automated check results |
| 0.3 | 2026-04-30 | Priyanshu | Updated after STP v0.3 polish; refreshed test-first baseline; recorded local backend/frontend smoke status; kept manual UI cases pending until browser evidence is available |
| 0.3 evidence update | 2026-05-02 | Priyanshu | Added browser evidence for AASX and IEC importer tests; recorded CD-Manager navigation failure and dependent blocked core tests |

---

## 1. Contribution Boundary

- **Implemented and documented by Priyanshu:** AASX CD Importer module and AASX CD Importer MOD.
- **Tested by Priyanshu as Test Manager:** CD-Manager core behavior, AASX CD Importer, IEC Importer, repository connectivity, validation, and error handling.
- **Implemented by another team member:** IEC Importer. Priyanshu tests the integrated behavior from the user perspective and documents the result here, but does not claim IEC implementation or IEC module documentation ownership.

---

## 2. Test Environment

| Component | Details |
|---|---|
| OS | macOS |
| Timezone | Europe/Berlin |
| Web UI branch tested | `concept-description-manager` |
| Web UI commit tested | `5621030` |
| Documentation repo branch | `main` |
| Documentation repo commit before this update | `71e0b9b` |
| Automated/local-stack baseline date | 2026-04-30 |
| Browser system-test execution date | 2026-05-02 |
| Backend URL | `http://localhost:8081` |
| Frontend URL | `http://127.0.0.1:3000` |
| Browser evidence status | AASX and IEC importer screenshots captured; CD-Manager core screenshots blocked because the CD-Manager navigation entry/view is unavailable |

---

## 3. Automated And Local Stack Evidence

Executed from `Team1-basyx-aas-web-ui/aas-web-ui` unless otherwise noted. Baseline refreshed on 2026-04-30 against the currently running local stack.

| Check | Result | Summary / Evidence |
|---|---|---|
| Docker backend stack | PASS | `docker compose ps` showed `cd-repo`, `aas-env`, `aas-registry`, `sm-registry`, and `aas-discovery` running healthy. |
| CD Repository endpoint | PASS | `curl -i http://127.0.0.1:8081/concept-descriptions` returned HTTP 200 with `{"paging_metadata":{},"result":[]}`. |
| Frontend HTTP smoke | PASS | The already-running frontend at `http://127.0.0.1:3000` returned HTTP 200 to `curl -I http://127.0.0.1:3000`. |
| `npm run lint:check` | FAIL | ESLint reported 3919 errors and 415 warnings. Main areas include `IecCddValidator.ts`, `IecFileImport.ts`, `IecImporter.vue`, `AasxCdImporter/index.vue`, `AASXImport.ts`, and related tests. |
| `npm run test:run` | FAIL | Vitest: 23 test files passed, 6 suites failed during import, 300 tests passed. Failure cause remains `TypeError: localStorage.getItem is not a function` from Vue devtools/Pinia initialization. |
| Focused IEC tests | PASS | `npm run test:run -- tests/composables/IecCddValidator.test.ts tests/composables/IecFileImport.test.ts`: 2 test files passed, 88 tests passed. Duration: 1.12s. |
| `npm run type-check` | PASS | `vue-tsc --noEmit` completed successfully. |
| `npm run build` | FAIL | Build failed in `prebuild` because `lint:check` exited with 1 before the test/build stages could complete. |
| `npm run build-only` | PASS | Vite production bundle compiled successfully in 3.37s. Warnings remain for browser-externalized `fs/promises`, large chunks, and ineffective dynamic imports. |

Interpretation:

- The local backend/frontend stack is running and answers basic HTTP requests.
- The TypeScript type surface is valid.
- The production bundle can be generated when bypassing the `prebuild` gate.
- The official build gate is not release-ready because lint and full Vitest automation fail.
- Manual browser execution is still required for the STP system test cases.

---

## 4. Manual System Test Status

Browser-based STP execution was performed on 2026-05-02. Importer test cases have explicit browser evidence. CD-Manager core test cases are failed or blocked because the current frontend navigation does not expose a CD-Manager / Concept Description Manager view.

| Metric | Count |
|---|---:|
| Required manual STP test cases | 16 |
| Passed in current merged-branch browser run | 7 |
| Failed in current merged-branch browser run | 1 |
| Blocked in current merged-branch browser run | 8 |
| Pending manual execution/evidence | 0 |
| Optional not executed | 1 |
| Previously passed on 2026-03-31 AASX branch | 3 |

---

## 5. Required Manual Test Case Results

| TC ID | Name | Current merged-branch result | Evidence / Next action |
|---|---|---|---|
| TC.NAV.001.001.F | Header navigation to CD-Manager | FAIL | Browser navigation dropdown does not contain a CD-Manager / Concept Description Manager entry. Evidence: `EV-TC-NAV-001-001-navigation-dropdown.png`, `EV-TC-NAV-001-001-modules-dropdown.png`. Defect: `DEF-CDM-001`. |
| TC.ATTR.002.001.F | Sidebar collapse and expand | BLOCKED | CD-Manager view is unavailable, so the CD-Manager sidebar cannot be opened or tested. Blocked by `DEF-CDM-001`. |
| TC.TBL.003.001.F | Attribute selection + max 4 columns | BLOCKED | CD-Manager view is unavailable, so attribute selection and max-column behavior cannot be tested. Blocked by `DEF-CDM-001`. |
| TC.TBL.003.002.F | Table pagination | BLOCKED | CD-Manager view is unavailable, so table pagination cannot be tested. Blocked by `DEF-CDM-001`. |
| TC.TBL.003.003.F | Free text search | BLOCKED | CD-Manager view is unavailable, so CD table search cannot be tested. Blocked by `DEF-CDM-001`. |
| TC.DETAIL.004.001.F | Detail view shows full CD | BLOCKED | CD-Manager table/detail workflow is unavailable. Blocked by `DEF-CDM-001`. |
| TC.DETAIL.004.002.F | Edit + Save + Persistence | BLOCKED | CD-Manager detail/edit workflow is unavailable. Blocked by `DEF-CDM-001`. |
| TC.DETAIL.004.003.F | Validation + duplicate detection | BLOCKED | CD-Manager create/edit validation workflow is unavailable. Blocked by `DEF-CDM-001`. |
| TC.CREATE.005.001.F | Create CD via popup manual form | BLOCKED | CD-Manager manual create workflow is unavailable. Blocked by `DEF-CDM-001`. |
| TC.AASX.006.001.F | Scan AASX file for CDs | PASS | `TestWithCDs.aasx` scan showed 3 CDs: Rotation Speed, Temperature, Manufacturer Name; all 3 were NEW. Evidence: `EV-TC-AASX-006-001-scan.png`. |
| TC.AASX.006.002.F | Import NEW CDs from AASX | PASS | Import completed successfully with `Imported 3/3 Concept Description(s) successfully`. Evidence: `EV-TC-AASX-006-002-import.png`. |
| TC.AASX.006.003.F | Re-scan shows EXISTS + re-import via PUT | PASS | Re-scan showed 0 NEW and 3 ALREADY EXISTS; re-import completed with 3/3 successful. Evidence: `EV-TC-AASX-006-003-exists.png`. |
| TC.AASX.006.004.F | Reject invalid or empty AASX input | PASS | Invalid AASX showed readable parse feedback: `invalid package format: invalid zip data` and `Failed to scan AASX file`; no raw stack trace was shown. Evidence: `EV-TC-AASX-006-004-invalid.png`. |
| TC.IEC.007.001.F | Import valid IEC file and show extracted properties | PASS | Valid IEC CSV was detected as CSV and showed 1 IEC-CDD property with IRDI `0112/2///61360_7#AAE664#007`, preferred name `Rated voltage`, unit `V`, and mapped data type `REAL_MEASURE`. Evidence: `EV-TC-IEC-007-001-valid-preview.png`. |
| TC.IEC.007.002.F | Reject invalid or non-IEC file content | PASS | Invalid IEC CSV was rejected with `The data does not match the IEC-CDD format`; save action was not offered and no crash occurred. Evidence: `EV-TC-IEC-007-002-invalid-warning.png`. |
| TC.IEC.007.003.F | Save IEC properties to CD Repository and handle missing repository URL | PASS | Missing repository URL showed a clear configuration error; configured URL save succeeded with `Successfully saved: 1 created, 0 updated.` Evidence: `EV-TC-IEC-007-003-missing-url.png`, `EV-TC-IEC-007-003-save.png`. |

### Optional Test Case

| TC ID | Name | Result | Notes |
|---|---|---|---|
| TC.TBL.003.004.F | Per-column filter and sort | Not executed | Optional SRS §3 feature; execute only if implemented in the final UI. |

---

## 6. Non-Functional Observations

| ID | Check | Current observation | Result |
|---|---|---|---|
| NFR-01 | Usability | Importer workflows provide visible success/error feedback. CD-Manager usability cannot be accepted because the required CD-Manager view is not reachable in navigation. | Fail / blocked for CD-Manager |
| NFR-02 | Responsive design | Desktop browser evidence at 1440x1000 was captured for navigation, AASX Importer, and IEC Importer. Dedicated 1366x768 and 1920x1080 comparison screenshots were not captured. | Partially observed |
| NFR-03 | Maintainability | Automated checks show maintainability risk because lint gate fails heavily and full Vitest is blocked by a test-environment import error; no final browser-console evidence captured yet. | Risk open |

---

## 7. Defect / Risk List

| ID | Severity | Area | Description | Evidence / Reference | Status |
|---|---|---|---|---|---|
| DEF-AUTO-001 | Major | Quality gate / lint | `npm run lint:check` fails with 3919 errors and 415 warnings. This blocks the official `npm run build` prebuild gate. | Automated check 2026-04-30 | Open |
| DEF-AUTO-002 | Major | Test automation | `npm run test:run` fails because 6 suites crash during import with `localStorage.getItem is not a function`. 23 test files and 300 tests still pass, so this appears to be a test-environment/setup issue that blocks full automated regression evidence. | Vitest output 2026-04-30 | Open |
| DEF-AUTO-003 | Major | Build gate | `npm run build` fails because `prebuild` stops at lint. Diagnostic `npm run build-only` passes, so bundling itself is possible. | Build output 2026-04-30 | Open |
| DEF-CDM-001 | Critical | CD-Manager availability | Required CD-Manager / Concept Description Manager view is not reachable from the current frontend navigation. The Modules list shows AAS Importer, AASX CD Importer, IEC Importer, Query Language, and other modules, but no CD-Manager entry. This fails `TC.NAV.001.001.F` and blocks the CD-Manager table/detail/create test cases. | Browser evidence 2026-05-02: `EV-TC-NAV-001-001-navigation-dropdown.png`, `EV-TC-NAV-001-001-modules-dropdown.png` | Open |
| RISK-IEC-001 | Medium | IEC requirement scope | Team objective describes IEC-CDD URL import, while the integrated IEC Importer currently presents file upload as the supported workflow. This should be clarified as accepted scope or documented as a requirement deviation. | STP v0.3 IEC scope note | Open |
| RISK-MANUAL-001 | Closed | System-test evidence | Browser-based STP evidence has been captured for all required cases as PASS, FAIL, or BLOCKED. Remaining risk is represented by `DEF-CDM-001` and the automated quality-gate defects. | Manual result table 2026-05-02 | Closed |

---

## 8. Evidence To Attach Before Final Submission

Recommended screenshot filenames:

| Area | Evidence filenames |
|---|---|
| Navigation/core | `EV-TC-NAV-001-001-root.png`, `EV-TC-NAV-001-001-navigation-dropdown.png`, `EV-TC-NAV-001-001-modules-dropdown.png`; remaining core screenshots are blocked by missing CD-Manager view |
| Table/detail/create | Blocked by `DEF-CDM-001`; no CD-Manager table/detail/create screenshots can be captured from the current UI |
| AASX | `EV-TC-AASX-006-001-scan.png`, `EV-TC-AASX-006-002-import.png`, `EV-TC-AASX-006-003-exists.png`, `EV-TC-AASX-006-004-invalid.png` |
| IEC | `EV-TC-IEC-007-001-valid-preview.png`, `EV-TC-IEC-007-002-invalid-warning.png`, `EV-TC-IEC-007-003-missing-url.png`, `EV-TC-IEC-007-003-save.png` |
| NFR | `EV-NFR-1366x768.png`, `EV-NFR-1920x1080.png`, browser-console screenshot if errors appear |

Manual IEC test data is available in `PROJECT/STP/test-data/iec-valid-property.csv` and `PROJECT/STP/test-data/iec-invalid-property.csv`.

Terminal evidence already captured in this STR:

- Docker backend healthy: `cd-repo`, `aas-env`, `aas-registry`, `sm-registry`, and `aas-discovery`.
- CD Repository HTTP 200 with empty result array.
- Frontend HTTP 200 at `http://127.0.0.1:3000`.
- Automated command results: `type-check` PASS, focused IEC tests PASS, full Vitest FAIL, `build-only` PASS, lint/build gate FAIL.
- Browser evidence captured with Chromium on 2026-05-02 for navigation, AASX Importer, and IEC Importer.

---

## 9. Conclusion

**Overall current result:** Completed with defects.

The STP structure is complete and aligned with the professor's testing guidance: requirements-based tests, traceability, equivalence classes, boundary values, separated test data, and clear test-case naming are present.

The local test environment was verified on 2026-04-30. Backend and frontend smoke checks passed, `npm run type-check` passed, focused IEC automated tests passed, and `npm run build-only` passed. Browser system tests were executed on 2026-05-02. The AASX CD Importer and IEC Importer integrated workflows passed their required STP cases.

The required CD-Manager core workflow is not acceptable in the current merged frontend because no CD-Manager / Concept Description Manager navigation entry or view is available. This causes one failed navigation test and blocks the dependent sidebar, table, detail, edit, validation, and create test cases. The official quality gate is also not clean because lint, full Vitest, and `npm run build` fail.

**Presentation message:** Priyanshu can show strong evidence for his owned AASX CD Importer and for IEC black-box integration testing, while transparently reporting the missing CD-Manager core view as an open critical defect.

---

*Prepared by: Priyanshu (Test Manager, Team 1)*
