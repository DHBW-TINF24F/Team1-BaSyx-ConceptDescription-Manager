# TINF24F_1-STP-0v2 — System Test Plan (STP)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)
**Team:** Team 1
**Role owner:** Priyanshu (Test Manager)
**Date:** 2026-03-31
**Status:** v0.2 — Revised to professor's standard format

---

## Version Control

| Version | Date       | Author    | Notes |
|---------|------------|-----------|-------|
| 0.1     | 2026-02-14 | Priyanshu | Initial draft |
| 0.2     | 2026-03-31 | Priyanshu | Revised: professor's TC naming format, tabular test case layout, Äquivalenzklassenanalyse, Grenzwertanalyse, separated test data tables, AASX CD Importer test cases added |

---

## 1. Purpose

This System Test Plan (STP) describes how the CD-Manager plugin is tested at the system level. Testing is performed from the user perspective (black-box), exercising end-to-end flows through the Web UI against a real BaSyx backend.

---

## 2. System Under Test (SUT)

- **SUT:** CD-Manager plugin inside BaSyx AAS Web UI
- **Backend:** Eclipse BaSyx ConceptDescription Repository (cd-repo)
- **Implemented features (from SRS + Semester 4):**
  - §1 Header navigation to CD-Manager
  - §2 Attribute selector sidebar (collapse/expand, attribute checkboxes)
  - §3 Data table (pagination, free text search, max 4 columns)
  - §4 Detail view (view, edit, save, validation, duplicate detection)
  - §5 Create CD via popup — manual form (required)
  - §6-AASX AASX CD Importer — new Semester 4 module (Priyanshu)

---

## 3. Test Scope

### 3.1 In Scope (required features)
- Header navigation to CD-Manager
- Sidebar collapse/expand and attribute selection
- Max 4 columns boundary enforcement
- Table pagination and free text search
- Detail view: full CD display, edit mode, save, persistence after reload
- Validation: required fields; duplicate detection
- Create CD via manual form
- AASX CD Importer: scan, import NEW, re-import EXISTS

### 3.2 Out of Scope
- Column filter/sort per column (SRS §3 — optional, not implemented)
- File drag-and-drop import (SRS §5 — optional)
- Clone full CD repository (SRS §6 — optional)

---

## 4. Test Strategy

**Primary strategy:** Requirements-based testing (Anforderungsbasiert) — mandatory minimum per professor's guidelines.

All test cases are derived directly from SRS requirements. Every SRS section has at least one corresponding test case (see Traceability Matrix, Section 11).

---

## 4a. Äquivalenzklassenanalyse

Equivalence classes are identified for key input dimensions. Each class is covered by at least one test case.

| Feature | Equivalence Classes — Valid | Equivalence Classes — Invalid |
|---|---|---|
| Column count (TC.TBL.003.001.F) | 1 col, 2 cols, 3 cols, 4 cols | 5th column (must be rejected) |
| Free text search (TC.TBL.003.003.F) | Known substring match, exact match | Empty string (no change to results) |
| Required field validation (TC.DETAIL.004.003.F) | All mandatory fields filled | One mandatory field empty, all fields empty |
| Duplicate detection (TC.DETAIL.004.003.F) | Unique `idShort` | Same `idShort` as existing CD |
| AASX scan input (TC.AASX.006.001.F) | Valid `.aasx` file containing ≥1 CD | Valid `.aasx` with 0 CDs; corrupted/non-AASX file |
| AASX import selection (TC.AASX.006.002.F) | All rows selected, partial selection | Zero rows selected (import button must be disabled) |

---

## 4b. Grenzwertanalyse

Boundary values are tested for numeric and range-bounded inputs.

| Boundary | Lower Bound | Upper Bound / Rejection |
|---|---|---|
| Column count | 1 column (minimum valid) | 4 columns (maximum valid); 5th column → rejected |
| Table pagination | Page 1 (no previous page) | Last page (no next page) |
| AASX CD count | 1 CD in package | 0 CDs → shows error message "No Concept Descriptions found" |
| Edit save activation | No changes (Save disabled) | Any single field change → Save button appears |

---

## 5. Test Environment

### 5.1 Setup

| Component | Details |
|---|---|
| OS | macOS |
| Browser | Chrome (record exact version in STR) |
| Frontend | Team1-basyx-aas-web-ui, `npm run dev`, URL: `http://localhost:3000` |
| Backend | BaSyx cd-repo via Docker Compose, URL: `http://localhost:8081` |

### 5.2 Preconditions Checklist

1. Start backend: `cd development/setupFiles && docker compose up -d`
2. Confirm healthy: `docker ps` shows cd-repo container as running
3. Start frontend: `cd aas-web-ui && npm install --legacy-peer-deps && npm run dev`
4. Open `http://localhost:3000` → Settings → Infrastructure → set CD Repository URL to `http://localhost:8081`
5. Verify CD-Manager view is accessible from navigation

---

## 6. Entry and Exit Criteria

### 6.1 Entry Criteria
- Backend container running and healthy
- Frontend loads without critical errors
- CD-Manager view opens from navigation

### 6.2 Exit Criteria
- All required test cases executed (TC.NAV through TC.AASX)
- All results documented in STR with Pass/Fail and evidence
- All critical/major bugs either fixed and re-tested, or documented with impact

---

## 7. Test Data

### TD.001 — Standard CDs for create and duplicate tests

| Dataset | idShort | displayName (en) | Use in |
|---------|---------|-----------------|--------|
| 01 | `TC_Length_CD` | Length | TC.CREATE.005.001.F — create |
| 02 | `TC_Width_CD` | Width | TC.TBL.003.003.F — search |
| 03 | `TC_Length_CD` | Length (dup) | TC.DETAIL.004.003.F — duplicate |

### TD.002 — AASX test file

| File | Location | CDs included |
|------|----------|-------------|
| `TestWithCDs.aasx` | `examples/CombinedExample/Infrastructure1/aas/` | 3 CDs: Rotation Speed, Temperature, Manufacturer Name |

### TD.003 — Boundary test: empty / invalid AASX

| Dataset | File | Expected |
|---------|------|----------|
| 01 | Valid `.aasx` with empty `conceptDescriptions: []` | Error message: no CDs found |
| 02 | A non-AASX file (e.g., `.json` renamed to `.aasx`) | Parse error shown |

---

## 8. Defect Management

Defects found during system test are created as GitHub Issues:

- **Title format:** `[BUG] <short description>`
- **Labels:** `bug`, `system-test`, plus area label (`ui`, `cd-repo`, `aasx-importer`)
- **Severity levels:**
  - Critical: system unusable / data corruption
  - Major: main feature broken (CRUD/import)
  - Minor: UI glitches, text issues, small usability
- Each bug references the failing test case ID (e.g., "Fails TC.DETAIL.004.002.F step 5")

---

## 9. Test Cases — CD-Manager Core Features

---

### TC.NAV.001.001.F — Header navigation to CD-Manager

| Field | Value |
|-------|-------|
| **Test case ID** | TC.NAV.001.001.F |
| **Name** | Header navigation to CD-Manager |
| **Req.-ID** | SRS.§1 |
| **Description** | This testcase verifies that the CD-Manager view is reachable via the existing header navigation and loads without errors. |

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open `http://localhost:3000` in browser | App loads; no error page |
| 2 | Locate the CD-Manager / Concept Descriptions entry in the header navigation | Entry is visible |
| 3 | Click the entry | CD-Manager view opens; table and sidebar are visible |
| 4 | Observe for connection errors | No error banner or connection alert shown |

---

### TC.ATTR.002.001.F — Sidebar collapse and expand

| Field | Value |
|-------|-------|
| **Test case ID** | TC.ATTR.002.001.F |
| **Name** | Attribute selector sidebar collapse and expand |
| **Req.-ID** | SRS.§2 |
| **Description** | This testcase verifies that the left attribute selector sidebar can be collapsed and expanded, and that the table area adjusts correctly. |

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open CD-Manager view | Sidebar is visible on the left |
| 2 | Click the collapse button on the sidebar | Sidebar collapses; table area becomes wider |
| 3 | Confirm no UI overlap or broken layout | Table is fully usable, no content hidden |
| 4 | Click expand/open button | Sidebar reappears in original position |

---

### TC.TBL.003.001.F — Attribute selection and max 4 columns boundary

| Field | Value |
|-------|-------|
| **Test case ID** | TC.TBL.003.001.F |
| **Name** | Attribute selection and max 4 columns boundary |
| **Req.-ID** | SRS.§2, SRS.§3 |
| **Description** | This testcase verifies that attribute checkboxes control table columns, and that the system enforces a maximum of 4 simultaneous columns. |

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | In the sidebar, select 1 attribute checkbox | 1 column appears in the table |
| 2 | Select a 2nd, 3rd, and 4th attribute | 4 columns appear; layout remains intact |
| 3 | Attempt to select a 5th attribute | 5th selection is prevented (checkbox disabled, toast shown, or auto-deselect) |
| 4 | Deselect one attribute | Column disappears; 4th checkbox becomes selectable again |

**Equivalence classes applied:** see Section 4a — Column count classes.
**Boundary applied:** see Section 4b — Column count boundary (4 max, 5 rejected).

---

### TC.TBL.003.002.F — Table pagination

| Field | Value |
|-------|-------|
| **Test case ID** | TC.TBL.003.002.F |
| **Name** | Table pagination |
| **Req.-ID** | SRS.§3 |
| **Description** | This testcase verifies that the CD table uses backend-driven pagination and that navigation between pages works correctly. |

**Precondition:** At least 2 pages of CDs exist in the repository. If not, create several CDs using TD.001 format with unique idShort values.

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open CD-Manager with multiple CDs present | First page of results shown |
| 2 | Click "Next page" control | Second page loads with different CD entries |
| 3 | Click "Previous page" control | First page content returns |
| 4 | Navigate to last page | No "Next" button or it is disabled |

**Boundary applied:** see Section 4b — Pagination boundaries.

---

### TC.TBL.003.003.F — Free text search in table columns

| Field | Value |
|-------|-------|
| **Test case ID** | TC.TBL.003.003.F |
| **Name** | Free text search in table columns |
| **Req.-ID** | SRS.§3 |
| **Description** | This testcase verifies that free text search filters the CD table correctly and that clearing the search restores the full list. |

**Test data:** TD.001 — Dataset 01 (`TC_Length_CD`) and Dataset 02 (`TC_Width_CD`) must exist.

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Ensure `idShort` column is visible | Column displayed |
| 2 | Type `TC_Length` in the search field | Only CDs with matching substring remain; `TC_Width_CD` disappears |
| 3 | Clear the search field | Full list restored |
| 4 | Search for `TC_Width` | Only `TC_Width_CD` is shown |

**Equivalence classes applied:** see Section 4a — Free text search classes.

---

### TC.DETAIL.004.001.F — Detail view shows full CD

| Field | Value |
|-------|-------|
| **Test case ID** | TC.DETAIL.004.001.F |
| **Name** | Detail view shows full Concept Description |
| **Req.-ID** | SRS.§4 |
| **Description** | This testcase verifies that clicking a row in the table opens the full Concept Description in the right detail sidebar, and that all fields match the selected item. |

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open CD-Manager; ensure at least one CD is in the table | Table has at least one row |
| 2 | Click any row | Right sidebar opens with detail view |
| 3 | Compare displayed `id` / `idShort` with the clicked row | Values match |
| 4 | Verify the detail view shows the full CD, not a partial subset | All CD fields (description, displayName, embeddedDataSpecifications, etc.) are shown |

---

### TC.DETAIL.004.002.F — Edit mode, save activation on change, and persistence

| Field | Value |
|-------|-------|
| **Test case ID** | TC.DETAIL.004.002.F |
| **Name** | Edit mode, save on change, and persistence after reload |
| **Req.-ID** | SRS.§4 |
| **Description** | This testcase verifies that edit mode is accessible, that the Save button only activates after a change is made, that saving succeeds, and that the change persists after a browser reload. |

**Test data:** Use an existing CD (e.g., TD.001 Dataset 01). Change `displayName (en)` to `Length-Edited`.

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open a CD in detail view | CD details shown |
| 2 | Switch to edit mode | Fields become editable |
| 3 | Observe Save button before making any change | Save button is disabled or not visible |
| 4 | Change `displayName (en)` to `Length-Edited` | Save button becomes enabled/visible |
| 5 | Click Save | Success feedback shown; no error |
| 6 | Reload the browser (F5) and reopen the same CD | `displayName (en)` still shows `Length-Edited` |

**Boundary applied:** see Section 4b — Save activation boundary.

---

### TC.DETAIL.004.003.F — Validation and duplicate detection

| Field | Value |
|-------|-------|
| **Test case ID** | TC.DETAIL.004.003.F |
| **Name** | Required field validation and duplicate CD detection |
| **Req.-ID** | SRS.§4, SRS.§5 |
| **Description** | This testcase verifies that saving a CD with empty required fields is blocked with a validation message, and that creating a CD with a duplicate identifier is detected and prevented or handled. |

**Test data:** TD.001 Dataset 03 (`TC_Length_CD` duplicate).

**Part A — Required field validation:**

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open create CD form or edit an existing CD in edit mode | Form visible |
| 2 | Clear a mandatory field (e.g., `idShort` or `id`) | Field appears empty |
| 3 | Click Save/Create | Validation error message displayed; save blocked; no CD created/modified |

**Part B — Duplicate detection:**

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Create a CD with `idShort = TC_Length_CD` (TD.001 Dataset 01) | CD created successfully |
| 2 | Attempt to create a second CD with same `idShort = TC_Length_CD` (TD.001 Dataset 03) | Duplicate detected; system prevents silent overwrite or shows conflict dialog |

**Equivalence classes applied:** see Section 4a — Required field and duplicate classes.

---

### TC.CREATE.005.001.F — Create CD via popup manual form

| Field | Value |
|-------|-------|
| **Test case ID** | TC.CREATE.005.001.F |
| **Name** | Create CD via popup manual entry form |
| **Req.-ID** | SRS.§5 |
| **Description** | This testcase verifies that the create/import popup opens, that switching to the manual form works, that a new CD can be created with required fields, and that it appears in the table. |

**Test data:** TD.001 Dataset 02 (`TC_Width_CD`).

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Click the Import/Create button in the sidebar | Popup/dialog opens |
| 2 | Click the button to switch to manual entry form | Form fields appear |
| 3 | Fill in required fields using TD.001 Dataset 02 | Fields accept input |
| 4 | Click Save/Create | CD is created; success message shown |
| 5 | Find `TC_Width_CD` in the table | Entry is visible in the table |
| 6 | Click the row to open detail view | All entered fields are correct |

---

## 10. Test Cases — AASX CD Importer (Semester 4 Module)

---

### TC.AASX.006.001.F — Scan AASX file for Concept Descriptions

| Field | Value |
|-------|-------|
| **Test case ID** | TC.AASX.006.001.F |
| **Name** | Scan AASX file and display CD preview table |
| **Req.-ID** | SRS.§6-AASX |
| **Description** | This testcase verifies that uploading a valid AASX file and clicking Scan correctly extracts all Concept Descriptions and displays them in the preview table with correct status badges. |

**Test data:** TD.002 — `TestWithCDs.aasx` (3 CDs: Rotation Speed, Temperature, Manufacturer Name).

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to AASX CD Importer in the navigation menu | Importer page loads |
| 2 | Use file picker to select `TestWithCDs.aasx` | File name shown in picker |
| 3 | Click "Scan for Concept Descriptions" | Loading indicator appears briefly |
| 4 | Observe the preview table | 3 rows displayed: Rotation Speed, Temperature, Manufacturer Name |
| 5 | Observe status badges (assuming repo is empty) | All 3 rows show status NEW (green) |
| 6 | Observe summary banner | Shows "3 Concept Description(s) found: 3 NEW, 0 ALREADY EXISTS" |
| 7 | Confirm all checkboxes are pre-selected | All 3 rows are checked by default |

**Equivalence classes applied:** see Section 4a — AASX scan input (valid file with CDs).
**Boundary applied:** see Section 4b — AASX CD count.

---

### TC.AASX.006.002.F — Import NEW Concept Descriptions from AASX

| Field | Value |
|-------|-------|
| **Test case ID** | TC.AASX.006.002.F |
| **Name** | Import all NEW Concept Descriptions via POST |
| **Req.-ID** | SRS.§6-AASX |
| **Description** | This testcase verifies that selected NEW CDs from a scanned AASX file are successfully imported into the CD Repository via POST, and that the result summary reports correct counts. |

**Precondition:** TC.AASX.006.001.F has been executed; scan result shows 3 NEW CDs; all rows selected.

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Deselect one CD row (e.g., Manufacturer Name) | Import button updates to "Import 2 Selected" |
| 2 | Re-select all rows | Import button shows "Import 3 Selected" |
| 3 | Click "Import 3 Selected Concept Descriptions" | Confirmation dialog opens |
| 4 | Review dialog content | Shows "3 will be created (NEW), 0 will be overwritten" |
| 5 | Click "Confirm Import" | Import executes; snackbar shown |
| 6 | Observe snackbar | Shows "3/3 Concept Description(s) imported successfully" |
| 7 | Navigate to CD-Manager table | All 3 CDs (Rotation Speed, Temperature, Manufacturer Name) appear in repository |

---

### TC.AASX.006.003.F — Re-scan shows EXISTS status and re-import uses PUT

| Field | Value |
|-------|-------|
| **Test case ID** | TC.AASX.006.003.F |
| **Name** | Re-scan after import shows EXISTS; re-import updates via PUT |
| **Req.-ID** | SRS.§6-AASX |
| **Description** | This testcase verifies that scanning the same AASX file after a successful import correctly identifies all CDs as already existing (EXISTS), and that re-importing them updates the repository via PUT without errors. |

**Precondition:** TC.AASX.006.002.F has been executed; 3 CDs are in the repository.

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | On the AASX CD Importer page, click Reset (or re-upload same file) | Page resets to idle state |
| 2 | Select `TestWithCDs.aasx` again and click Scan | Scan executes |
| 3 | Observe status badges | All 3 rows show EXISTS (orange/yellow) |
| 4 | Observe summary banner | Shows "3 NEW → 0, EXISTS → 3" |
| 5 | Click "Import 3 Selected Concept Descriptions" | Confirmation dialog opens |
| 6 | Review dialog content | Shows "0 will be created, 3 will be overwritten (EXISTS)" and overwrite warning |
| 7 | Click "Confirm Import" | Import executes |
| 8 | Observe result summary | Shows "3/3 imported" with no errors; CDs updated in repository |

---

## 11. Optional Test Cases (execute only if feature is present)

### TC.TBL.003.004.F (Optional) — Column filter and sort

| Field | Value |
|-------|-------|
| **Test case ID** | TC.TBL.003.004.F |
| **Name** | Per-column filter and sort (optional feature) |
| **Req.-ID** | SRS.§3 (optional) |
| **Description** | This testcase verifies that per-column sorting and filtering work correctly if the feature is implemented. If not implemented, mark "Not executed (optional not implemented)". |

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Click the sort icon on a column header | Column sorts ascending/descending |
| 2 | Apply a per-column filter (if available) | Results narrow to matches only |

---

## 12. Non-Functional Checks

These are observations noted during test execution, not formal pass/fail test cases.

| ID | Check | Criteria |
|----|-------|---------|
| NFR-01 | Usability (SRS.NFR.1) | Navigation is intuitive; error messages are human-readable (no raw stack traces); UI is consistent with BaSyx style |
| NFR-02 | Responsive design (SRS.NFR.2) | Tested at 1366×768 and 1920×1080; no overlapping elements or broken sidebars |
| NFR-03 | Maintainability (SRS.NFR.3) | No excessive console errors during normal use |

---

## 13. Traceability Matrix (SRS → Test Cases)

| SRS Section | Topic | Test Cases |
|---|---|---|
| SRS.§1 | Header navigation | TC.NAV.001.001.F |
| SRS.§2 | Attribute selector + collapse | TC.ATTR.002.001.F, TC.TBL.003.001.F |
| SRS.§3 | Table: pagination + search + max 4 cols | TC.TBL.003.001.F, TC.TBL.003.002.F, TC.TBL.003.003.F |
| SRS.§4 | Detail view + edit + save + validation | TC.DETAIL.004.001.F, TC.DETAIL.004.002.F, TC.DETAIL.004.003.F |
| SRS.§5 | Create CD (required part) | TC.CREATE.005.001.F |
| SRS.§6-AASX | AASX CD Importer (Semester 4) | TC.AASX.006.001.F, TC.AASX.006.002.F, TC.AASX.006.003.F |
| SRS.NFR.1 | Usability | NFR-01 |
| SRS.NFR.2 | Responsive design | NFR-02 |
| SRS.NFR.3 | Maintainability | NFR-03 |

---

## 14. Notes

- Screenshots are sufficient evidence for UI-level test steps.
- For backend errors, include browser console screenshot + docker logs:
  `docker compose -f development/setupFiles/docker-compose.yaml logs -f cd-repo`
- After test execution, results are recorded in the STR (`TINF24F_1-STR-0v1.md`).
- Bugs found during testing are filed as GitHub Issues following the format in Section 8.
