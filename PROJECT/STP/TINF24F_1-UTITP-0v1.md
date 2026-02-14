# TINF24F_1-UTITP-0v1 — Test Plan for Unit & Integration Tests (UT/IT)
**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)  
**Team:** Team 1  
**Author:** Priyanshu (Software Tester / Test Manager)  
**Date:** 2026-02-14  
**Status:** v0.1 (planning phase, no implementation yet)

---

## 0. Why this document exists (Issue alignment)
This document is created to fulfill the issue requirement:

**Ziel:** Testplan für Unit- und Integrationstests erstellen  
**Aufgaben:**
1) overview of which components/modules are needed and developed  
2) for each module write test cases  
3) for implementing tests, search the Eclipse BaSyx (our web UI repo) for existing tests and align with them

This is a **plan only** (first version). Implementation can follow later if needed.

---

## 1. Test levels (short + practical)
### 1.1 Unit tests
- test single functions/composables/modules in isolation
- no real backend required
- network calls are mocked
- focus: correctness + edge cases

### 1.2 Integration tests
- test multiple parts together (e.g., composable + client, or component + composable)
- can be done with mocked HTTP OR with a real running `cd-repo` container
- focus: correct data flow and correct usage of the cd-repo API

*(System tests are planned separately in STP/STR for lecturer artifacts.)*

---

## 2. Existing tests in our BaSyx Web UI repo (required by issue)
### 2.1 What I checked
In the frontend repo (`Team1-basyx-aas-web-ui/aas-web-ui`) tests already exist under:

- `tests/`

Example test files currently present:
- `tests/composables/AAS/ReferableUtils.test.ts`
- `tests/composables/IDUtils.test.ts`
- `tests/utils/StringUtils.test.ts`
- `tests/utils/ObjectUtils.test.ts`
- `tests/utils/ObjectUtils.test.ts`
- `tests/utils/AAS/SemanticIdUtils.test.ts`

### 2.2 Conclusion (how we align)
- existing naming convention: `*.test.ts`
- test focus currently: **utils** and **composables**
- the project uses **Vitest** (confirmed in `package.json`), plus `@testing-library/vue` is available if we later want component tests

So for our CD-Manager tests:
- we will first write tests for **client + composable logic**, because that matches the existing repo style.
- component tests are optional and only planned for critical flows.

---

## 3. Overview: components/modules needed + developed (Issue task #1)
Based on repo search (real paths), ConceptDescription related code is mainly here:

### 3.1 Client / API layer
- `src/composables/Client/CDRepositoryClient.ts`  
Purpose: communication with the ConceptDescription Repository (`cd-repo`) for list/get/create/update/delete.

### 3.2 ConceptDescription logic layer
- `src/composables/AAS/ConceptDescriptionHandling.ts`  
Purpose: logic around ConceptDescriptions (fetching, mapping, formatting helpers).  
Evidence: referenced in multiple places like `SubmodelElements/Property.ts` and `SMEHandling.ts`.

### 3.3 UI components related to CDs (for later integration tests)
- `src/components/UIComponents/ConceptDescription.vue`
- `src/components/UIComponents/DescriptionElement.vue`
- `src/components/UIComponents/DescriptionTooltip.vue`
- `src/components/UIComponents/GenericDataTableView.vue`

### 3.4 Plugin system usage
- concept descriptions are supported via plugin mechanism (example reference found):
  - `src/UserPlugins/HelloWorldPlugin.vue` mentions `withConceptDescriptions`

---

## 4. Test scope (UT/IT)
### 4.1 In scope
- correctness of cd-repo client behavior (request + error handling)
- ConceptDescriptionHandling logic correctness (no crashes, correct formatting/mapping)
- basic validation and duplicate logic if implemented in CD-Manager code
- minimal integration tests between client + handling (+ optionally one component)

### 4.2 Out of scope (for this UT/IT plan)
- full browser E2E automation (not required by issue, and no cypress/playwright found yet)
- load/performance testing
- security testing beyond basic error handling

---

## 5. Unit test cases per module (Issue task #2)
> Unit tests should be fast and run without docker/backend.

### Module A — CDRepositoryClient (`src/composables/Client/CDRepositoryClient.ts`)
**Planned UT test cases:**
- **UT-A1:** List ConceptDescriptions returns parsed data (happy path)
- **UT-A2:** List supports pagination parameters (if cursor/page params exist)
- **UT-A3:** Get single CD by id (happy path)
- **UT-A4:** Create CD sends correct payload / method
- **UT-A5:** Update CD sends correct payload / method
- **UT-A6:** Delete CD calls correct endpoint and handles 404
- **UT-A7:** Error handling: backend returns non-OK (e.g., 500) → function throws/returns controlled error

**Mocking plan:** mock `fetch` (or the used http layer) using Vitest mocks.

**Planned test file location (aligned to repo):**
- `tests/composables/Client/CDRepositoryClient.test.ts`

---

### Module B — ConceptDescriptionHandling (`src/composables/AAS/ConceptDescriptionHandling.ts`)
**Planned UT test cases:**
- **UT-B1:** Composable returns expected public API (sanity check)
- **UT-B2:** Handles empty list / undefined input safely (no crash)
- **UT-B3:** Formatting/mapping helpers return stable output for minimal CD object
- **UT-B4:** Unit/symbol helper returns expected suffix (if function exists, used in Property.ts)
- **UT-B5:** If it fetches CDs via client: correct behavior when client returns 0 CDs and >0 CDs (client mocked)

**Planned test file location:**
- `tests/composables/AAS/ConceptDescriptionHandling.test.ts`

---

### Module C — UI helper logic (only if extracted)
If CD-Manager validation or duplicate detection exists as separate helpers (utils), we test it as utils.

**Planned UT test cases:**
- **UT-C1:** Required-field validation fails if idShort empty/whitespace
- **UT-C2:** Required-field validation passes with minimal valid data
- **UT-C3:** Duplicate detection returns true when idShort already exists
- **UT-C4:** Duplicate detection returns false when unique

**Planned test file location:**
- `tests/utils/CDManager/ValidationUtils.test.ts`
- `tests/utils/CDManager/DuplicateUtils.test.ts`

*(If validation is only inside components, we will do a small component test instead of utils test.)*

---

## 6. Integration test cases (Issue task #2)
> Integration tests combine multiple parts. We keep them minimal (MVP).

### IT-01 — Client + Handling (mocked HTTP)
**Modules:** CDRepositoryClient + ConceptDescriptionHandling  
**Idea:** mock HTTP response in client and verify handling transforms/returns correct output without errors.  
**Expected:** correct data is passed through and helper outputs are consistent.

### IT-02 — Table selection → detail view update (optional)
**Modules:** GenericDataTableView + ConceptDescription component  
**Idea:** render component(s) with test data and simulate selecting an entry.  
**Expected:** detail view receives correct object and displays correct fields.

*(Only planned if component setup is stable; otherwise we keep integration at composable level.)*

### IT-03 — Minimal real-backend integration (optional/local)
**Precondition:** `cd-repo` container running on `http://localhost:8081`  
**Idea:** verify one real end-to-end call set works locally:
- list → create (TC prefix) → list contains → delete cleanup  
**Expected:** backend persists and UI/client handles real responses.

Note: This might be run locally only (depends on CI availability and stability).

---

## 7. Test data strategy
- Use unique prefixes to avoid collisions:
  - `UT_...` for unit tests (mostly mocked anyway)
  - `IT_...` for integration tests
  - `TC_...` for any real-backend records
- If we create data on real backend, we always delete it in cleanup.

---

## 8. Where tests will live (aligned with repo)
Because existing tests are under `tests/utils` and `tests/composables`, our plan follows the same structure:

Planned new folders/files:
- `tests/composables/Client/CDRepositoryClient.test.ts`
- `tests/composables/AAS/ConceptDescriptionHandling.test.ts`
Optional (if needed):
- `tests/utils/CDManager/ValidationUtils.test.ts`
- `tests/utils/CDManager/DuplicateUtils.test.ts`
- `tests/components/UIComponents/...` (only if we decide to test Vue components directly)

Naming stays `*.test.ts`.

---

## 9. How to run tests (from package.json scripts)
Run from: `Team1-basyx-aas-web-ui/aas-web-ui`

- all tests:
  - `yarn test`
- watch mode:
  - `yarn test:watch`
- coverage:
  - `yarn test:coverage`
- prebuild gate (recommended before merging):
  - `yarn prebuild`

---

## 10. Priority (realistic order)
If we implement tests later, this is the order:

1) **CDRepositoryClient tests** (UT-A) — because if API calls are wrong, everything breaks.
2) **ConceptDescriptionHandling tests** (UT-B) — used in multiple areas (property/unit logic).
3) **Validation + duplicates** tests (UT-C) — required by SRS for saving/creating.
4) **Integration test IT-01** with mocked client — good sanity integration.
5) Optional: IT-03 against real backend (local).

---

## 11. Notes / open points
- Optional features (sorting/filtering, file import, clone) will only get tests if they exist in implementation.
- If we discover additional existing patterns/mocks in `tests/`, we will reuse them instead of inventing a new style.
- A separate STP/STR (system test plan/report) is still required for the lecturer deliverables; this document only targets the UT/IT issue.
