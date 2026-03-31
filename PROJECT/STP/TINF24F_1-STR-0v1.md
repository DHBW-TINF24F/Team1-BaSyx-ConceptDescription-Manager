# TINF24F_1-STR-0v1 — System Test Report (STR)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)
**Team:** Team 1
**Role owner:** Priyanshu (Test Manager)
**Date:** 2026-03-31
**Status:** In progress — partial execution completed

---

## Version Control

| Version | Date | Author | Notes |
|---------|------|--------|-------|
| 0.1 | 2026-03-31 | Priyanshu | Template created; results to be filled after execution |

---

## 1. Test Environment (record exact versions)

| Component | Details |
|---|---|
| OS | macOS |
| Browser | Chrome (exact version not recorded) |
| Frontend URL | `http://localhost:3001` |
| Backend URL | `http://localhost:8081` |
| Frontend commit / branch | not recorded |
| Docker image (cd-repo) | `eclipsebasyx/conceptdescription-repository:2.0.0-SNAPSHOT` |
| Test execution date | 2026-03-31 |

---

## 2. Test Execution Summary

| Metric | Count |
|---|---|
| Total test cases | 12 |
| Passed | 3 |
| Failed | 0 |
| Not executed | 9 |
| Defects found | 0 |

---

## 3. Test Case Results

### 3.1 Required Test Cases

| TC ID | Name | Result | Actual Result / Notes | Evidence | Bug ref |
|---|---|---|---|---|---|
| TC.NAV.001.001.F | Header navigation to CD-Manager | Not executed | Not executable in the tested build on 2026-03-31. A dedicated `CD-Manager` navigation entry was not available in the running frontend. | - | - |
| TC.ATTR.002.001.F | Sidebar collapse and expand | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD-Manager sidebar UI was not available. | - | - |
| TC.TBL.003.001.F | Attribute selection + max 4 columns | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD-Manager table UI was not available. | - | - |
| TC.TBL.003.002.F | Table pagination | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD-Manager table UI was not available. | - | - |
| TC.TBL.003.003.F | Free text search | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD-Manager table UI was not available. | - | - |
| TC.DETAIL.004.001.F | Detail view shows full CD | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD detail view was not available. | - | - |
| TC.DETAIL.004.002.F | Edit + Save + Persistence | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD edit/save UI was not available. | - | - |
| TC.DETAIL.004.003.F | Validation + duplicate detection | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD create/edit UI was not available. | - | - |
| TC.CREATE.005.001.F | Create CD via popup manual form | Not executed | Not executable in the tested build on 2026-03-31 because the corresponding CD manual creation popup was not available. | - | - |
| TC.AASX.006.001.F | Scan AASX file for CDs | PASS | `TestWithCDs.aasx` was uploaded successfully. After clicking `Search for Concept Descriptions`, the system displayed 3 Concept Descriptions: Rotation Speed, Temperature, and Manufacturer Name. All three rows were pre-selected. In this environment, all 3 were shown as `EXISTS` / `ALREADY EXISTS` instead of `NEW`, indicating the repository already contained the records. No error occurred during valid scan. | Preview table screenshot | - |
| TC.AASX.006.002.F | Import NEW CDs from AASX | PASS | NEW import scenario was executed successfully during testing. Import of selected NEW Concept Descriptions worked correctly and the importer behavior matched the expected workflow for creating new records from an AASX file. | Import dialog / success message screenshot | - |
| TC.AASX.006.003.F | Re-scan shows EXISTS + re-import via PUT | PASS | Re-import scenario executed successfully. Confirmation dialog showed: `NEW 0 will be created`; `EXISTS 3 will be updated (overwritten)`. After confirmation, the system displayed: `Import complete: 3/3 Concept Description(s) imported successfully`. Additional boundary checks passed: with 0 selected rows the import button was disabled; with 1 selected row import worked correctly. Invalid file test with `invalid-test-file.aasx` showed `invalid package format: invalid zip data` and `Failed to scan AASX file` without crashing. | Overwrite dialog + success message screenshots | - |

### 3.2 Optional Test Cases

| TC ID | Name | Result | Notes |
|---|---|---|---|
| TC.TBL.003.004.F | Column filter and sort | Not executed | Optional feature not implemented / not available in current build |

---

## 4. Non-Functional Observations

| ID | Check | Observation | Result |
|----|-------|-------------|--------|
| NFR-01 | Usability | AASX CD Importer workflow was understandable. Error messages for invalid AASX input were readable and no raw stack traces were shown in the UI. | OK |
| NFR-02 | Responsive design | Not systematically tested during this execution session. | Not evaluated |
| NFR-03 | Maintainability | During tested AASX flows, no excessive visible runtime errors blocked usage. | OK |

---

## 5. Defect List

No defects found during executed AASX importer tests.

---

## 6. AASX CD Importer — Test Evidence Notes

*(Document specific observations for the new AASX CD Importer module)*

**TC.AASX.006.001.F — Scan result:**
- Screenshot of preview table with 3 CDs
- Observed status in this environment: `0 NEW`, `3 ALREADY EXISTS`

**TC.AASX.006.002.F — Import result:**
- NEW import scenario executed successfully
- Screenshot of confirmation dialog: *(attach)*
- Screenshot of success message after import: *(attach)*

**TC.AASX.006.003.F — Re-scan result:**
- Screenshot of preview table with `EXISTS` badges: *(attach)*
- Screenshot of confirmation dialog with overwrite warning: *(attach)*
- Screenshot of success message: `Import complete: 3/3 Concept Description(s) imported successfully`
- Additional notes:
  - with zero selected rows, import button disabled
  - with one selected row, import succeeded
  - invalid `.aasx` file produced parse error and failed scan message without app crash

---

## 7. Conclusion

**Overall test result:** PARTIAL / IN PROGRESS

**Summary:**
Executed AASX CD Importer tests passed in the current environment. Valid scan, NEW import, overwrite import, zero-selection boundary, one-selection import, and invalid-file error handling behaved correctly.

**Outstanding issues:**
Most non-AASX test cases were not executable in the tested build on 2026-03-31 and therefore remain unexecuted.

**Recommendation:**
Continue execution of remaining mandatory system tests for features that are available in the current build.

---

*Prepared by: Priyanshu (Test Manager, Team 1)*
