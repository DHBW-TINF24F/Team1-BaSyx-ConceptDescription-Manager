# Test Manager Final Presentation Notes

**Speaker:** Priyanshu  
**Role:** Test Manager / Software Tester  
**Own module contribution:** AASX CD Importer implementation and MOD  
**Testing contribution:** STP, STR, test design, automated check evidence, manual system-test planning, defect/risk reporting

---

## Slide 1 — Role and Contribution

Use this message:

> My role was Test Manager / Software Tester. I also implemented and documented the AASX CD Importer module. For modules implemented by other team members, such as the IEC Importer, my contribution was testing the integrated behavior from the user perspective and documenting the result.

Show:

- AASX CD Importer = implemented/documented by Priyanshu
- STP/STR = owned by Priyanshu as Test Manager
- IEC Importer = tested by Priyanshu, implemented by teammate

---

## Slide 2 — Testing Approach

Use this message:

> I followed the V-model idea from the lecture: requirements from the SRS are mapped into the STP, then execution results are documented in the STR.

Mention professor-required methods:

- Requirements-based system testing as minimum strategy
- Traceability from requirement to test case
- Equivalence classes
- Boundary value analysis
- Test instructions separated from test data
- Defects documented instead of silently fixed

Good example:

| Requirement | Test design method | Example |
|---|---|---|
| Max 4 table columns | Boundary value analysis | 4 columns accepted, 5th rejected |
| Importer validates input | Equivalence classes | Valid AASX/IEC file vs invalid/non-IEC file |

---

## Slide 3 — Test Coverage

Show a small coverage matrix:

| Area | Covered by |
|---|---|
| CD-Manager navigation/list/detail | Planned in TC.NAV, TC.TBL, TC.DETAIL; blocked in final browser run because CD-Manager view is not reachable |
| Create/edit/validation | Planned in TC.CREATE, TC.DETAIL; blocked by missing CD-Manager view |
| AASX CD Importer | TC.AASX.006.001-004 passed |
| IEC Importer integration | TC.IEC.007.001-003 passed |
| NFR checks | Usability, responsiveness, maintainability |
| Automated support | lint, Vitest, type-check, build |

Say clearly:

> IEC is included for system coverage because it is part of the integrated product, but I do not claim its implementation.

---

## Slide 4 — Results, Defects, and Outlook

Final browser system-test status from 2026-05-02:

| Result category | Count |
|---|---:|
| Required STP cases | 16 |
| Passed | 7 |
| Failed | 1 |
| Blocked | 8 |

Use this message:

> The AASX CD Importer and IEC Importer integration tests passed in the browser. The CD-Manager core view was not reachable from the current frontend navigation, so I documented it as a critical defect instead of marking the dependent tests as passed.

Current automated/local-stack status from 2026-04-30:

| Check | Result |
|---|---|
| `npm run lint:check` | Failed |
| `npm run test:run` | Failed: 300 tests passed, 6 suites failed during import |
| IEC focused tests | Passed: 88 tests |
| `npm run type-check` | Passed |
| `npm run build` | Failed because prebuild stops at lint |
| `npm run build-only` | Passed |
| Docker backend + frontend smoke | Passed |

Defects/risks to mention only if still open:

- Critical: CD-Manager / Concept Description Manager view is not reachable from the current frontend navigation.
- Lint gate currently blocks official build.
- Vitest setup has a `localStorage.getItem` issue.
- IEC Importer currently supports file upload, while the original objective describes URL import; this should be clarified as accepted scope or future work.

Suggested closing:

> The main value of my testing work was making the system status transparent: what is covered, what passed, what still needs evidence, and which risks remain before final delivery.

---

## Demo Priority

If there is time for only one tester demo, prioritize:

1. AASX CD Importer scan/import/re-import, because this is Priyanshu's owned module.
2. One invalid-input case, because it shows negative testing.
3. IEC Importer only as integrated system coverage, not as owned implementation.

Best screenshots for slides:

1. `EV-TC-AASX-006-001-scan.png` — valid AASX scan with 3 Concept Descriptions.
2. `EV-TC-AASX-006-002-import.png` — successful 3/3 import.
3. `EV-TC-AASX-006-003-exists.png` — re-scan detects existing CDs.
4. `EV-TC-AASX-006-004-invalid.png` — invalid AASX rejected with readable error.
5. `EV-TC-IEC-007-001-valid-preview.png` — IEC black-box preview evidence.
6. `EV-TC-NAV-001-001-modules-dropdown.png` — evidence for missing CD-Manager navigation entry, only if discussing defects.
