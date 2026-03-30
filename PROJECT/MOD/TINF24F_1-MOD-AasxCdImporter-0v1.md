# TINF24F_1-MOD-0v1 — Module Documentation (MOD)

**Project:** BaSyx ConceptDescription-Plugin (CD-Manager)
**Team:** Team 1
**Role owner:** Priyanshu (Test Manager)
**Date:** 2026-03-30
**Status:** Draft v0.1

---

## Table of Contents

- [Version Control](#version-control)
- [1. Introduction](#1-introduction)
  - [1.1 Purpose](#11-purpose)
  - [1.2 Scope](#12-scope)
  - [1.3 Definitions, Acronyms, Abbreviations](#13-definitions-acronyms-abbreviations)
- [2. Module: AASX CD Importer](#2-module-aasx-cd-importer)
  - [2.1 Overview](#21-overview)
  - [2.2 User Flow](#22-user-flow)
  - [2.3 Architecture & File Structure](#23-architecture--file-structure)
  - [2.4 Key Components](#24-key-components)
  - [2.5 Data Flow](#25-data-flow)
  - [2.6 Error Handling](#26-error-handling)
- [3. Setup & Run Instructions](#3-setup--run-instructions)
  - [3.1 Prerequisites](#31-prerequisites)
  - [3.2 Start the Backend](#32-start-the-backend)
  - [3.3 Start the Frontend](#33-start-the-frontend)
  - [3.4 Configure the CD Repository URL](#34-configure-the-cd-repository-url)
- [4. Test File](#4-test-file)
- [5. Known Limitations](#5-known-limitations)

---

## Version Control

| Version | Date       | Author    | Notes   |
|---------|------------|-----------|---------|
| 0.1     | 2026-03-30 | Priyanshu | Draft 1 |

---

## 1. Introduction

### 1.1 Purpose

This Module Documentation (MOD) describes the **AASX CD Importer** feature implemented as part of the BaSyx CD-Manager plugin. It explains what the module does, how it is structured, how to run it, and how it fits into the overall system.

The intended readers are developers, testers, and project owners who want to understand, extend, or test this specific feature.

### 1.2 Scope

This document covers the AASX CD Importer module only. It does not describe other CD-Manager features such as the table view, detail view, or IEC-CDD URL importer. Those are covered separately in the SRS and SAS.

The AASX CD Importer was added in response to a new requirement raised during Semester 4 implementation:

> *"Eine AASX wird geladen — Der Importer scannt die enthaltenen CDs und EDSs — Der Importer macht Mappingvorschläge auf existierende CDs im Zielrepository — Oder legt entsprechende CDs im Zielrepository an."*

### 1.3 Definitions, Acronyms, Abbreviations

| Term | Meaning |
|------|---------|
| AASX | Asset Administration Shell Exchange format — a ZIP-based package format containing AAS, Submodels, and Concept Descriptions |
| CD | Concept Description — a standardized metadata definition in the AAS metamodel |
| CDD | Common Data Dictionary — a standardized catalog of CDs (e.g. IEC CDD, ECLASS) |
| EDS | Embedded Data Specification — the IEC 61360 content block inside a CD |
| IEC 61360 | International standard for data element types used in CDs |
| ECLASS | Industry classification standard using IRDI-format semantic IDs |
| IRDI | International Registration Data Identifier — e.g. `0173-1#02-AAO677#002` |
| POST | HTTP method used to create a new resource |
| PUT | HTTP method used to update an existing resource |

---

## 2. Module: AASX CD Importer

### 2.1 Overview

The AASX CD Importer is a frontend module page inside the BaSyx AAS Web UI. It allows users to upload an `.aasx` file and import the Concept Descriptions (CDs) and Common Data Dictionaries (CDDs) it contains directly into the configured BaSyx CD Repository.

All other content inside the AASX package — AAS Shells and Submodels — is completely ignored. Only `conceptDescriptions` entries from the AAS environment are processed.

The module appears automatically in the Web UI navigation under the name **"AASX CD Importer"** and is available on desktop only.

### 2.2 User Flow

```
1. User selects an .aasx file using the file picker
        ↓
2. User clicks "Scan for Concept Descriptions"
        ↓
3. App extracts all CDs from the AASX package
        ↓
4. App fetches existing CDs from the target CD Repository
        ↓
5. App compares IDs and assigns status to each CD:
   - NEW    → CD does not exist in the repository yet
   - EXISTS → CD with same ID already exists in the repository
        ↓
6. A preview table is shown with all extracted CDs,
   their Preferred Name, ID, and status badge.
   All rows are pre-selected. User can deselect individual rows.
        ↓
7. User clicks "Import N Selected Concept Descriptions"
        ↓
8. A confirmation dialog shows a breakdown:
   - How many will be created (POST)
   - How many will be overwritten (PUT)
   - Warning if any existing CDs will be overwritten
        ↓
9. User confirms → import is executed per CD:
   - NEW    → POST /concept-descriptions
   - EXISTS → PUT  /concept-descriptions/{base64(id)}
        ↓
10. Result summary shown in-page and as a snackbar notification
```

### 2.3 Architecture & File Structure

The feature consists of two files:

```
aas-web-ui/
├── src/
│   ├── composables/
│   │   └── AAS/
│   │       └── AASXImport.ts              ← MODIFIED
│   └── pages/
│       └── modules/
│           └── AasxCdImporter/
│               └── index.vue              ← NEW
└── examples/
    └── CombinedExample/
        └── Infrastructure1/
            └── aas/
                └── TestWithCDs.aasx       ← NEW (test file)
```

The module is auto-registered by the Vue Router at the route `/modules/aasxcdimporter` — no manual route configuration is needed. This follows the same pattern as all other modules in the `pages/modules/` directory.

### 2.4 Key Components

#### `extractCdsFromAasx(file: File)` — `AASXImport.ts`

A new exported async function added to the existing `AASXImport.ts` composable. It opens an AASX package using `aasx-package-ts`, iterates the AAS environment specs, and collects only `conceptDescriptions` entries. AAS Shells and Submodels are not touched.

**Signature:**
```typescript
export async function extractCdsFromAasx(file: File): Promise<ExtractCdsResult>

export type ExtractCdsResult = {
    cdById: Map<string, { core: aasCore.types.ConceptDescription; json: JsonRecord }>;
    warnings: string[];
};
```

- Returns a `Map` keyed by CD ID for fast lookup
- Any CD that fails validation by `aas-core3.1-typescript` is skipped with a warning instead of crashing the whole import
- The existing `useAASXImport()` composable and its behavior are completely unchanged

#### `AasxCdImporter/index.vue`

The Vue 3 module page component. Handles the full UI lifecycle:

| Phase | Description |
|-------|-------------|
| `idle` | Waiting for file selection |
| `scanning` | Parsing AASX and fetching existing CDs from repo |
| `ready` | Table shown, user reviewing and selecting rows |
| `importing` | Sending POST/PUT requests per selected row |
| `done` | Results displayed |

#### `useCDRepositoryClient()` — reused from existing code

All REST communication reuses the existing `CDRepositoryClient.ts` composable:
- `fetchCdList()` — fetches all existing CDs for comparison
- `postConceptDescription(cd)` — creates a new CD
- `putConceptDescription(cd)` — updates an existing CD

### 2.5 Data Flow

```
InfrastructureStore
  └── getConceptDescriptionRepoURL
          ↓
  CDRepositoryClient
    ├── fetchCdList()       → Set of existing CD IDs
    ├── postConceptDescription()  → POST new CDs
    └── putConceptDescription()   → PUT existing CDs

  extractCdsFromAasx(file)
    └── aasx-package-ts → parseEnvironmentText → parseConceptDescription
          ↓
        Map<id, {core, json}>

  AasxCdImporter/index.vue
    ├── Merge: extracted IDs vs existing IDs → CdRow[] with status
    ├── Table: user selects rows
    ├── Confirm dialog
    └── executeImport() → POST/PUT per row → importSummary
```

### 2.6 Error Handling

| Scenario | Behaviour |
|----------|-----------|
| Corrupt or unreadable AASX | Error caught in `scanFile()`, displayed as red alert, phase stays `idle` |
| AASX contains no CDs | Info message: "No Concept Descriptions found", phase stays `idle` |
| Single CD fails to parse (invalid `dataType` etc.) | Skipped with a warning entry, other CDs are still processed |
| `fetchCdList()` fails (repo unreachable) | Warning shown, all extracted CDs treated as NEW |
| POST or PUT fails for a specific CD | Tracked in `importErrors`, reflected in the final summary count |
| CD Repository URL not configured | `postConceptDescription` / `putConceptDescription` return `false` immediately, counted as failed |
| User selects zero rows | Import button is disabled |

---

## 3. Setup & Run Instructions

### 3.1 Prerequisites

- Node.js ≥ 18
- npm ≥ 9
- Docker Desktop (for the BaSyx backend)
- Git

### 3.2 Start the Backend

```bash
cd Team1-BaSyx-ConceptDescription-Manager/development/setupFiles
docker compose up -d
```

This starts the following services:

| Service | Port | Description |
|---------|------|-------------|
| cd-repo | 8081 | BaSyx CD Repository |
| aas-env | 9081 | AAS Environment |
| aas-registry | 9082 | AAS Registry |
| sm-registry | 9083 | Submodel Registry |
| aas-discovery | 9084 | AAS Discovery |

> **Note:** The CD Repository uses in-memory storage by default. All data is lost when the container is stopped.

### 3.3 Start the Frontend

```bash
cd Team1-basyx-aas-web-ui/aas-web-ui
npm install --legacy-peer-deps
npm run dev
```

The app is then available at `http://localhost:3000`.

### 3.4 Configure the CD Repository URL

1. Open `http://localhost:3000`
2. Go to **Settings → Infrastructure**
3. Set **Concept Description Repository** URL to: `http://localhost:8081`
4. Save

Then navigate to **AASX CD Importer** in the navigation menu.

---

## 4. Test File

A ready-made test AASX file is included in the repository specifically for testing this feature:

```
examples/CombinedExample/Infrastructure1/aas/TestWithCDs.aasx
```

It contains 3 Concept Descriptions covering the most common real-world formats:

| ID | Preferred Name | DataType | Unit |
|----|---------------|----------|------|
| `https://admin-shell.io/test/cd/RotationSpeed/1/0` | Rotation Speed | `INTEGER_MEASURE` | 1/min |
| `https://admin-shell.io/test/cd/Temperature/1/0` | Temperature | `REAL_MEASURE` | °C |
| `0173-1#02-AAO677#002` | Manufacturer Name (ECLASS) | `STRING` | — |

All three use the IEC 61360 data specification template with `preferredName` in English and German.

---

## 5. Known Limitations

- **In-memory storage only:** The default Docker setup does not persist CDs to disk. A database-backed BaSyx configuration is needed for permanent storage.
- **No batch progress indicator:** During import of large AASX files with many CDs, there is no per-item progress bar — only a final summary.
- **ID-based matching only:** The mapping between extracted CDs and existing repository CDs is done by exact ID match. Semantic similarity matching (e.g. equivalent IRDI formats) is not yet implemented.
- **Single file at a time:** The file picker accepts only one AASX file per import session. Batch import of multiple AASX files is not supported.
