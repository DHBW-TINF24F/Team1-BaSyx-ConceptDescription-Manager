# Applications

- Frontend (Vue)
- Backend (Java Spring)
- Database (MongoDB)
- Identity Provider (Keycloak)

# Context

Modules are one implementation of a tiny feature in the frontend.
This can be a single file or multiple files.

# Terms

- CD - Concept Description
- SM - Submodel
- AAS - Asset Administration Shell
- FE - Frontend
- BE - Backend
- DB - Database
- IDP - Identity Provider
- JWT - JSON Web Token

# Modules

1. CD Table
2. CD Store (only manages the selected item in frontend)
3. Interaction menu
4. CD Detail view
5. CD Editor
6. Reference Module (Creates or removes references) (two modes: "table-action", "dependency-conflict")
7. Reference Checker (Checks which SMs reference CDs)
8. CD JSON exporter
9. CD JSON importer
10. CD Deletion view
11. IEC CDD Importer
12. AASX CD Importer (Ignores everything from the AAS, which is neiter a CD, nor an embedded CD)
13. Diff module

# Communication Flows

1. List find and search CDs
2. View CD
3. Edit CD
4. Reference/Dereference CD for specific SM
5. Export CD
6. Import CD
7. Delete CD (not referenced CD)
8. Delete CD (referenced CD)
9. AASX CD Import
10. IEC CDD Import

# Communication Flow descriptions

Here I mainly cover frontend Modules as defined above, for the other Applications I will not go into internal details.

# Frontend to Backend Communitation Flow

This will always be referenced as "FE to BE Flow"

1. Requester to Client Module
2. Client module adds JWT if required
3. Client sends request to BE
4. BE sends JWT to IDP
5. IDP verifies JWT and returns to BE
6. BE queries DB
7. DB returns result to BE
8. BE returns result to Client
9. Client returns result to Requester

## List find and search CDs:

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data

# View CD

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data
6. Click on CD in table
7. CD Table updates Selected CD in CD Store
8. Interaction menu opens
9. Click on "View" action
10. Interaction menu gives CD from CD store to Detail view
11. Detail view displays Data

# Edit CD

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data
6. Click on CD in table
7. CD Table updates Selected CD in CD Store
8. Interaction menu opens
9. Click on "Edit" action
10. Interaction menu gives CD from CD store to CD Editor
11. CD Editor displays Data
12. User changes data
    1. On Cancel: close CD Editor without changes
    2. End of flow
13. On Save: FE to BE Flow
14. On success: Editor passes updated CD to CD Table
15. On failure: Editor displays error

# Reference/Dereference CD for specific SM

1. Open Web UI
2. Select SM
3. AAS store updated selected SM
4. Select CD Manager (CD Table)
    1. FE to BE Flow
5. Click on search bar
6. Enter search parameter
    1. FE to BE Flow
7. CD Table displays data
8. Click on CD in table
9. CD Table updates Selected CD in CD Store
10. Interaction menu gives SM and CD to reference checker
11. Interaction menu opens
12. Click on "Reference or "Dereference" action depending if the SM references the CD
13. Interaction menu gives CD and SM from stores to Reference Model
14. Reference Module displays Data in "table-action" mode
15. On cancel: Close Reference Module
    1. End of flow
16. On save: add/remove reference
    1. FE to BE Flow
17. On success: Update in AAS
18. On Failure: show error message

# Export CD

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data
6. Click on CD in table
7. CD Table updates Selected CD in CD Store
8. Interaction menu opens
9. Click on "Export" action
10. . Interaction menu gives CD from CD store to CD JSON exporter
11. CD JSON Exporter opens OS file manager
12. User selects folder and changes file name if needed
13. On cancel: close file manager
    1. end of flow
14. On success: download file

# Import CD

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on upload button
4. Importer opens
5. User selects json import
6. importer opens OS file manager
7. user selects json
8. importer imports json
    1. FE to BE Flow
9. On failure: display error message
10. On success: close importer

# Delete CD (not referenced CD)

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data
6. Click on CD in table
7. CD Table updates Selected CD in CD Store
8. Click on "Delete" action
9. CD Delete view opens in delete mode
10. CD Delete view passes CD to reference checker
    1. no references found variation
11. On cancel: close CD Delete view
12. On Delete: FE to BE Flow
    1. On Success: close CD Delete view
    2. On Failure: show error message

# Delete CD (referenced CD)

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on search bar
4. Enter search parameter
    1. FE to BE Flow
5. CD Table displays data
6. Click on CD in table
7. CD Table updates Selected CD in CD Store
8. Click on "Delete" action
9. Detail view opens in delete mode
10. Detail view passes CD to reference checker
    1. references found variation
    2. Reference Module opens in "dependency-conflict" mode
    3. On cancel: Reference module closed
    4. user inputs confirm text to remove all references
    5. user accepts with button click
        1. triggers: FE to BE Flow to delete references
11. On cancel: close Detail view
12. On Delete: FE to BE Flow
    1. On Success: close Detail view
    2. On Failure: show error message

# AASX CD Import

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on upload button
4. Importer opens
5. User selects AASX import
6. AASX importer opens
7. AASX importer opens OS file manager
8. user selects file
9. AASX file manger displays table of found CDs
    1. FE to BE Flow to check if some already exist
10. if view button clicked: open detail view for CD
11. if diff button clicked: open diff view for CD
12. On Cancel: close importer
13. On import: import data
    1. On failure: display failure message
    2. on success: display success message
    3. end of flow

# IEC CDD Import

1. Open Web UI
2. Select CD Manager (CD Table)
    1. FE to BE Flow
3. Click on upload button
4. Importer opens
5. User selects IEC CDD import
6. IEC CDD importer opens
7. IEC CDD importer opens OS file manager
8. user selects file
9. IEC CDD file manger displays table of found CDs
    1. FE to BE Flow to check if some already exist
10. if view button clicked: open detail view for CD
11. if diff button clicked: open diff view for CD
12. On Cancel: close importer
13. On import: import data
    1. On failure: display failure message
    2. on success: display success message
    3. end of flow
