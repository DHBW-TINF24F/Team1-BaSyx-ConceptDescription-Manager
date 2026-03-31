# TINF24F_1-STR-0v1 — System Test Report (STR)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)
**Team:** Team 1
**Role owner:** Priyanshu (Test Manager)
**Date:** *(fill in date of test execution)*
**Status:** Draft — awaiting test execution

---

## Version Control

| Version | Date | Author | Notes |
|---------|------|--------|-------|
| 0.1 | 2026-03-31 | Priyanshu | Template created; results to be filled after execution |

---

## 1. Test Environment (record exact versions)

| Component | Details |
|---|---|
| OS | *(e.g., macOS 15.x)* |
| Browser | *(e.g., Chrome 135.0.x)* |
| Frontend URL | `http://localhost:3000` |
| Backend URL | `http://localhost:8081` |
| Frontend commit / branch | *(e.g., feature/aasx-cd-importer @ abc1234)* |
| Docker image (cd-repo) | *(e.g., eclipsebasyx/basyx-repository:latest)* |
| Test execution date | *(fill in)* |

---

## 2. Test Execution Summary

| Metric | Count |
|---|---|
| Total test cases | 12 |
| Passed | *(fill in)* |
| Failed | *(fill in)* |
| Not executed (optional) | *(fill in)* |
| Defects found | *(fill in)* |

---

## 3. Test Case Results

### 3.1 Required Test Cases

| TC ID | Name | Result | Actual Result / Notes | Evidence | Bug ref |
|---|---|---|---|---|---|
| TC.NAV.001.001.F | Header navigation to CD-Manager | *(PASS / FAIL)* | *(describe actual behavior if different from expected)* | *(screenshot ref)* | *(GitHub issue # if any)* |
| TC.ATTR.002.001.F | Sidebar collapse and expand | *(PASS / FAIL)* | | | |
| TC.TBL.003.001.F | Attribute selection + max 4 columns | *(PASS / FAIL)* | | | |
| TC.TBL.003.002.F | Table pagination | *(PASS / FAIL)* | | | |
| TC.TBL.003.003.F | Free text search | *(PASS / FAIL)* | | | |
| TC.DETAIL.004.001.F | Detail view shows full CD | *(PASS / FAIL)* | | | |
| TC.DETAIL.004.002.F | Edit + Save + Persistence | *(PASS / FAIL)* | | | |
| TC.DETAIL.004.003.F | Validation + duplicate detection | *(PASS / FAIL)* | | | |
| TC.CREATE.005.001.F | Create CD via popup manual form | *(PASS / FAIL)* | | | |
| TC.AASX.006.001.F | Scan AASX file for CDs | *(PASS / FAIL)* | | | |
| TC.AASX.006.002.F | Import NEW CDs from AASX | *(PASS / FAIL)* | | | |
| TC.AASX.006.003.F | Re-scan shows EXISTS + re-import via PUT | *(PASS / FAIL)* | | | |

### 3.2 Optional Test Cases

| TC ID | Name | Result | Notes |
|---|---|---|---|
| TC.TBL.003.004.F | Column filter and sort | *(PASS / FAIL / Not executed)* | *(If not implemented: "Not executed — optional feature not implemented")* |

---

## 4. Non-Functional Observations

| ID | Check | Observation | Result |
|----|-------|-------------|--------|
| NFR-01 | Usability | *(Describe: navigation intuitive? error messages readable? UI consistent?)* | *(OK / Issue noted)* |
| NFR-02 | Responsive design | *(Tested resolutions: 1366×768, 1920×1080 — any overlap or broken layout?)* | *(OK / Issue noted)* |
| NFR-03 | Maintainability | *(Any excessive console errors during normal use?)* | *(OK / Issue noted)* |

---

## 5. Defect List

*(List all bugs found during testing. If none, write "No defects found.")*

| Bug # | GitHub Issue | TC ID | Severity | Description | Status |
|---|---|---|---|---|---|
| 1 | *(#xx)* | *(TC.xxx)* | *(Critical / Major / Minor)* | *(short description)* | *(Open / Fixed / Won't fix)* |

---

## 6. AASX CD Importer — Test Evidence Notes

*(Document specific observations for the new AASX CD Importer module)*

**TC.AASX.006.001.F — Scan result:**
- Screenshot of preview table with 3 CDs and NEW badges: *(attach)*

**TC.AASX.006.002.F — Import result:**
- Screenshot of confirmation dialog: *(attach)*
- Screenshot of success snackbar (3/3): *(attach)*
- Screenshot of CDs appearing in CD-Manager table: *(attach)*

**TC.AASX.006.003.F — Re-scan result:**
- Screenshot of preview table with EXISTS badges: *(attach)*
- Screenshot of confirmation dialog with overwrite warning: *(attach)*

---

## 7. Conclusion

*(Fill in after all tests are executed)*

**Overall test result:** *(PASS / CONDITIONAL PASS / FAIL)*

**Summary:**
*(Example: "All 12 required test cases passed. 1 minor bug found (see Bug #1) — does not block release. Optional feature TC.TBL.003.004.F not implemented and not executed.")*

**Outstanding issues:**
*(List any open bugs or known limitations that affect the product)*

**Recommendation:**
*(Example: "The CD-Manager plugin including the AASX CD Importer is ready for submission. All mandatory features work as specified in the SRS.")*

---

*Prepared by: Priyanshu (Test Manager, Team 1)*
