# TINF24F_1-STR-0v1 — System Test Report (STR)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)
**Team:** Team 1
**Role owner:** Priyanshu (Test Manager)
**Date:** 2026-03-31
**Status:** Partial — remaining tests blocked pending branch merge

---

## Version Control

| Version | Date | Author | Notes |
|---------|------|--------|-------|
| 0.1 | 2026-03-31 | Priyanshu | Initial execution — AASX CD Importer tests completed; remaining tests blocked pending merge of other feature branches |

---

## 1. Test Environment

| Component | Details |
|---|---|
| OS | macOS |
| Browser | Chrome |
| Frontend URL | `http://localhost:3001` |
| Backend URL | `http://localhost:8081` |
| Frontend branch tested | `feature/aasx-cd-importer` |
| Docker image (cd-repo) | `eclipsebasyx/conceptdescription-repository:2.0.0-SNAPSHOT` |
| Test execution date | 2026-03-31 |

---

## 2. Test Execution Summary

| Metric | Count |
|---|---|
| Total test cases | 12 |
| Passed | 3 |
| Failed | 0 |
| Not executed (blocked) | 9 |
| Defects found | 0 |

---

## 3. Test Case Results

### 3.1 Required Test Cases

| TC ID | Name | Result | Actual Result / Notes | Evidence | Bug ref |
|---|---|---|---|---|---|
| TC.NAV.001.001.F | Header navigation to CD-Manager | Not executed | The CD-Manager navigation entry and its UI (sidebar, table, detail view) are implemented on separate feature branches (`feature/cdeditDialogAndValidation`, `feature/6/IEC-Importer`) that have not yet been merged into the testable build. Test will be executed once those branches are merged. | — | — |
| TC.ATTR.002.001.F | Sidebar collapse and expand | Not executed | Blocked — CD-Manager sidebar not available in current build. See note above. | — | — |
| TC.TBL.003.001.F | Attribute selection + max 4 columns | Not executed | Blocked — CD-Manager table not available in current build. See note above. | — | — |
| TC.TBL.003.002.F | Table pagination | Not executed | Blocked — CD-Manager table not available in current build. See note above. | — | — |
| TC.TBL.003.003.F | Free text search | Not executed | Blocked — CD-Manager table not available in current build. See note above. | — | — |
| TC.DETAIL.004.001.F | Detail view shows full CD | Not executed | Blocked — CD-Manager detail view not available in current build. See note above. | — | — |
| TC.DETAIL.004.002.F | Edit + Save + Persistence | Not executed | Blocked — CD-Manager edit/save UI not available in current build. See note above. | — | — |
| TC.DETAIL.004.003.F | Validation + duplicate detection | Not executed | Blocked — CD-Manager create/edit UI not available in current build. See note above. | — | — |
| TC.CREATE.005.001.F | Create CD via popup manual form | Not executed | Blocked — CD-Manager create popup not available in current build. See note above. | — | — |
| TC.AASX.006.001.F | Scan AASX file for CDs | PASS | `TestWithCDs.aasx` uploaded successfully. After clicking "Scan for Concept Descriptions", the system displayed 3 Concept Descriptions: Rotation Speed, Temperature, and Manufacturer Name. All 3 rows were pre-selected. Status showed 0 NEW / 3 ALREADY EXISTS, indicating the repository already contained these records from a prior test run. No errors occurred. | Preview table screenshot | — |
| TC.AASX.006.002.F | Import NEW CDs from AASX | PASS | NEW import scenario executed successfully. Importing selected NEW Concept Descriptions worked correctly. The importer behavior matched the expected workflow for creating new records from an AASX file via POST. | Import dialog / success message screenshot | — |
| TC.AASX.006.003.F | Re-scan shows EXISTS + re-import via PUT | PASS | Re-import scenario executed successfully. Confirmation dialog correctly showed: "0 NEW will be created, 3 EXISTS will be updated (overwritten)" with overwrite warning. After confirmation: "Import complete: 3/3 Concept Description(s) imported successfully". Additional boundary checks passed: zero selected rows → import button disabled; one selected row → import succeeded correctly. Invalid `.aasx` file produced "invalid package format: invalid zip data" and "Failed to scan AASX file" without crashing the application. | Overwrite dialog + success message screenshots | — |

### 3.2 Optional Test Cases

| TC ID | Name | Result | Notes |
|---|---|---|---|
| TC.TBL.003.004.F | Column filter and sort | Not executed | Optional feature — not implemented in current build |

---

## 4. Non-Functional Observations

| ID | Check | Observation | Result |
|----|-------|-------------|--------|
| NFR-01 | Usability | AASX CD Importer workflow was clear and intuitive. Error messages for invalid AASX input were readable with no raw stack traces shown to the user. | OK |
| NFR-02 | Responsive design | Not tested in this session — blocked by unavailability of CD-Manager UI in the current build. Will be evaluated once remaining branches are merged. | Not evaluated |
| NFR-03 | Maintainability | No excessive console errors observed during the AASX import test flows. | OK |

---

## 5. Defect List

No defects found during the executed AASX CD Importer tests.

---

## 6. AASX CD Importer — Test Evidence

**TC.AASX.006.001.F — Scan result:**
- Preview table displayed 3 CDs correctly: Rotation Speed, Temperature, Manufacturer Name
- Status observed: 0 NEW / 3 ALREADY EXISTS (repository already populated from prior run)
- Screenshot: *(attach)*

**TC.AASX.006.002.F — Import NEW result:**
- NEW import executed correctly via POST
- Screenshot of confirmation dialog: *(attach)*
- Screenshot of success snackbar: *(attach)*

**TC.AASX.006.003.F — Re-scan and overwrite result:**
- All 3 rows correctly showed EXISTS status on re-scan
- Confirmation dialog showed overwrite warning as expected
- Success message: "Import complete: 3/3 Concept Description(s) imported successfully"
- Zero-selection boundary: import button correctly disabled
- One-selection import: succeeded correctly
- Invalid file: parse error shown cleanly without app crash
- Screenshot of EXISTS preview table: *(attach)*
- Screenshot of overwrite confirmation dialog: *(attach)*
- Screenshot of success message: *(attach)*

---

## 7. Conclusion

**Overall test result:** PARTIAL — in progress

**Summary:**
3 out of 12 test cases were executed. All 3 AASX CD Importer test cases (TC.AASX.006.001.F, TC.AASX.006.002.F, TC.AASX.006.003.F) passed without any defects. The remaining 9 test cases covering the core CD-Manager features (navigation, sidebar, table, detail view, create popup) could not be executed because those features are implemented on separate feature branches that have not yet been merged into a testable build.

**Outstanding issues:**
- TC.NAV.001.001.F through TC.CREATE.005.001.F remain unexecuted pending merge of `feature/cdeditDialogAndValidation` and `feature/6/IEC-Importer` branches into `concept-description-manager`

**Recommendation:**
The AASX CD Importer module (implemented by Priyanshu) is fully tested and functioning correctly. Once the remaining feature branches are merged, Priyanshu will complete the remaining system test cases and update this report to STR v0.2.

---

*Prepared by: Priyanshu (Test Manager, Team 1)*
