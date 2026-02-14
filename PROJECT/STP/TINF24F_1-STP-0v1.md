# TINF24F_1-STP-0v1 — System Test Plan (STP)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)  
**Team:** Team 1  
**Role owner:** Priyanshu (Software Tester / Test Manager)  
**Date:** 2026-02-14  
**Status:** Draft for System Test Execution

---

## 1. Purpose of this document
This STP describes how we will perform the **system test** for the CD-Manager plugin (black-box).  
Goal is to prove that the implemented features from the SRS work end-to-end with the existing BaSyx ConceptDescription Repository.

System test here means: “real user workflow” + “plugin integrated in the Web UI” + “backend reachable”.

---

## 2. System under test (SUT)
**SUT:** CD-Manager plugin inside BaSyx AAS Web UI  
**Backend:** Eclipse BaSyx ConceptDescription Repository (cd-repo)  
**Main features (from SRS):**
- Navigate to CD-Manager via header (required)
- Attribute selector sidebar (required)
- Table: pagination + free text search + max 4 columns (required); filter/sort optional
- Detail view: view + edit + save on change + validation + duplicate detection (required)
- Popup: create CD (required); file import optional
- Cloning from external CD repo optional

---

## 3. Test scope

### 3.1 In scope (required)
- Header navigation to CD-Manager
- Sidebar collapse/expand
- Attribute selection and “max 4 columns” behavior
- Table pagination
- Free text search in columns
- Detail view showing full CD for selected row
- Edit mode, save behavior, persistence after reload
- Validation: required fields
- Duplicate detection (as implemented)
- Create CD via popup → manual form

### 3.2 Out of scope (only if not implemented / optional)
- Column filter/sort per column (SRS says optional)
- File import via drag & drop (optional)
- Clone full CD repository (optional)

*(Note: If any optional feature **is** implemented, it will be tested and included as “Optional executed”.)*

---

## 4. Test approach (black-box + a bit of structure)
- **Black-box testing** from user perspective in the Web UI.
- Focus on end-to-end flows: UI → backend → persistence.
- We try to include:
  - **Equivalence classes:** valid vs invalid input, empty vs non-empty repository, etc.
  - **Boundary tests:** max 4 columns; empty required fields; duplicate cases.
- Defects are tracked in GitHub Issues.

---

## 5. Test environment

### 5.1 Local environment (target)
- OS: macOS
- Browser: Chrome and/or Safari (record exact versions in STR)
- Backend: `cd-repo` via Docker Compose
  - URL: `http://localhost:8081`
  - Container health must be “healthy”
- Frontend: Team1-basyx-aas-web-ui (local npm dev server)
  - URL: `http://localhost:<frontend-port>` (written in STR)

### 5.2 Preconditions / Setup checklist
Before system test execution:
1. Start cd-repo container:
   - `docker-compose -f development/setupFiles/docker-compose.yaml up -d`
2. Confirm it is healthy:
   - `docker ps` or `docker-compose ps`
3. Start frontend repo (`Team1-basyx-aas-web-ui`):
   - `npm install` (first time)
   - `npm start`
4. Open the UI and verify CD-Manager view is reachable.

---

## 6. Entry criteria / Exit criteria

### 6.1 Entry criteria (when we start system testing)
- Backend container is running and healthy
- Frontend loads without major errors
- CD-Manager view opens from header navigation

### 6.2 Exit criteria (when system test is “done”)
- All required test cases executed (TC-01 to TC-09)
- All results documented in STR with evidence
- All critical/major bugs have either:
  - been fixed and re-tested, **or**
  - are clearly documented with impact and workaround

---

## 7. Test data
To avoid conflicts with existing records, all manually created test records will use a **TC prefix**.

### 7.1 Test CDs (examples)
We create these (or similar) via the UI:

**CD #A**
- idShort: `TC04_Length_CD`
- displayName (en): `Length`
- description (en): `System test CD for length`

**CD #B**
- idShort: `TC05_Width_CD`
- displayName (en): `Width`
- description (en): `System test CD for width`

**CD #C (multilang)**
- idShort: `TC05_ML_CD`
- displayName (en): `Length`
- displayName (de): `Länge`
- description (en): `english text`
- description (de): `deutscher text`

### 7.2 Duplicate test record
- idShort: `TC08_DUPLICATE_CD`
(used twice to trigger duplicate behavior)

### 7.3 Optional Import/Clone test data
- If IEC import exists in the UI: use the URL from project docs (same one used by the team).
- If file import exists: prepare one valid JSON CD file + one invalid file type.

---

## 8. Defect management (GitHub Issues)
Defects found during system test are created as GitHub issues with:
- Title format: `[BUG] <short description>`
- Labels: `bug`, `system-test`, plus area label like `ui`, `cd-repo`, `plugin`
- Severity:
  - **Critical:** system unusable / data corruption
  - **Major:** main feature broken (CRUD/import)
  - **Minor:** UI glitches, text issues, small usability
- Each bug must reference the failing test case ID, e.g. “Fails TC-07 step 4”.
- Evidence attached (screenshots, console logs, request/response if needed).

---

## 9. Test cases (system test)

> Note: “Expected result” is what should happen. “Actual result” is documented later in STR.

### TC-01 — Header navigation to CD-Manager (Required)
**SRS reference:** Functional Req §1  
**Preconditions:** Frontend running.  
**Steps:**
1. Open the BaSyx Web UI in the browser.
2. Use the existing header navigation.
3. Click the navigation entry for Concept Descriptions / CD Manager.
**Expected result:**
- CD-Manager view opens successfully.
- No connection error is shown.

---

### TC-02 — Sidebar collapse/expand (Required)
**SRS reference:** Functional Req §2  
**Preconditions:** CD-Manager view open.  
**Steps:**
1. Locate the left sidebar (attribute selector).
2. Click “collapse” button.
3. Verify the table area becomes wider.
4. Expand sidebar again.
**Expected result:**
- Sidebar collapses/expands cleanly.
- Table remains usable, no overlap.

---

### TC-03 — Attribute selection + max 4 columns boundary (Required)
**SRS reference:** Functional Req §2 and §3  
**Preconditions:** CD-Manager open, table visible.  
**Steps:**
1. Select 1 attribute checkbox → verify 1 corresponding column appears.
2. Select total 4 attributes → verify 4 columns appear.
3. Try selecting a 5th attribute.
**Expected result:**
- Up to 4 columns are displayed.
- Selecting the 5th is prevented OR handled clearly (e.g., warning or auto-deselect).
- No broken layout.

---

### TC-04 — Pagination from backend (Required)
**SRS reference:** Functional Req §3  
**Preconditions:** Repository contains enough CDs to show more than one page.  
*(If not, create several CDs quickly with different idShort values.)*  
**Steps:**
1. Open table view.
2. Check pagination controls (next/prev/page number).
3. Go to next page.
4. Go back to previous page.
**Expected result:**
- Table content changes per page.
- Navigation works without errors.

---

### TC-05 — Free text search in table columns (Required)
**SRS reference:** Functional Req §3  
**Preconditions:** At least two CDs exist with different searchable values.  
**Steps:**
1. Ensure `idShort` column is visible.
2. Search for `TC04_Length` (or a known substring).
3. Verify results list filters to matching CDs.
4. Switch to another visible column (e.g. displayName) and search for `Width`.
**Expected result:**
- Search works in all tested columns.
- Non-matching entries disappear.
- Clearing search restores list.

---

### TC-06 — Detail view shows full selected CD (Required)
**SRS reference:** Functional Req §4  
**Preconditions:** Table has at least one CD entry.  
**Steps:**
1. Click a row in the table.
2. Observe the right sidebar detail view.
**Expected result:**
- Detail view shows the **full** ConceptDescription for the selected row (not only partial fields).
- Values match the selected item.

---

### TC-07 — Edit mode + Save appears on change + persistence (Required)
**SRS reference:** Functional Req §4  
**Preconditions:** A CD exists; detail view visible.  
**Test data:** change displayName/en to a new value like `Length-Edited`.  
**Steps:**
1. Open an existing CD in detail view.
2. Switch to edit mode.
3. Change one field.
4. Confirm Save button appears/enables after change.
5. Click Save.
6. Refresh browser (F5) and re-open same CD.
**Expected result:**
- Save only activates after changes.
- Data is saved successfully.
- After reload, changes still exist.

---

### TC-08 — Validation + duplicate detection (Required)
**SRS reference:** Functional Req §4 and §5  
**Part A — Required fields validation**  
**Steps:**
1. Try to create a CD but leave a mandatory field empty (e.g., idShort empty).
2. Click Save/Create.
**Expected result:**
- Validation message is shown.
- Save is blocked.
- No new CD is created.

**Part B — Duplicate detection**  
**Steps:**
1. Create a CD with idShort `TC08_DUPLICATE_CD`.
2. Attempt to create another CD with same idShort (or same unique field used in your app).
**Expected result:**
- Duplicate is detected.
- System prevents duplicate OR provides controlled conflict handling (as implemented).
- No silent overwrite.

---

### TC-09 — Create CD via popup → switch to manual form (Required)
**SRS reference:** Functional Req §5 (required part)  
**Preconditions:** CD-Manager open.  
**Steps:**
1. Click the “Import/Create” button in the sidebar.
2. Verify popup opens.
3. Use the button to switch the popup into manual create form.
4. Fill required fields (use test data TC prefix).
5. Create/save.
**Expected result:**
- Popup and form switch works.
- Created CD appears in list.
- Re-open CD in detail view to confirm fields stored.

---

## 10. Optional test cases (execute only if feature exists)

### TC-10 (Optional) — Column filter and sort (Optional feature)
**SRS reference:** Functional Req §3 (optional)  
**Steps:**
1. Use sort on one column.
2. Apply a filter (if UI has per-column filter).
**Expected result:**
- Sort works correctly.
- Filter narrows results correctly.
*(If feature not present: mark “Not executed (optional not implemented)”.)*


---

## 11. Non-functional checks (short)
These are not full test cases, but they are checked during execution.

### NFR-01 Usability (SRS NFR 1)
- Navigation is understandable.
- Error messages are readable (no stack traces).
- Main flows feel consistent with existing BaSyx UI style.

### NFR-02 Responsive design (SRS NFR 2)
- Test at two resolutions (e.g., 1366×768 and 1920×1080).
- No UI overlap; sidebars still usable.

### NFR-03 Maintainability (SRS NFR 3) — quick observation
- Not a black-box test, but we note if code conventions are followed where visible (e.g., no messy console spam).
- If CI exists, record if pipeline passes (optional note).

---

## 12. Traceability matrix (SRS → Test Cases)

| SRS Section | Topic | Test Cases |
|---|---|---|
| §1 | Header navigation to CD-Manager | TC-01 |
| §2 | Attribute selector + collapse/expand | TC-02, TC-03, TC-09 |
| §3 | Table pagination + search (+ optional filter/sort) | TC-04, TC-05, TC-10 |
| §4 | Detail view + edit + save + validation + duplicates | TC-06, TC-07, TC-08 |
| §5 | Create required, file import optional | TC-09, TC-11 |
| §6 | Cloning from CD repository optional | TC-12 |
| NFR 1 | Usability | NFR-01 (notes) |
| NFR 2 | Responsive design | NFR-02 (notes) |
| NFR 3 | Maintainability | NFR-03 (notes) |

---

## 13. Notes / small practical things
- For test evidence, screenshots are enough for UI steps.
- For backend errors, add browser console screenshot + docker logs if needed:
  - `docker-compose -f development/setupFiles/docker-compose.yaml logs -f`
- After system test, we create STR as separate document and fill Pass/Fail + evidence + bug links.

---