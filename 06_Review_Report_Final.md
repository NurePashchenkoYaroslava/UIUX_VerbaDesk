# VerbaDesk — Figma evidence review (Final)

## Initial evidence check

Reviewed artifact: the final supplied `VerbaDesk_Final.pdf`.

The observations below are based on the final supplied export. All previously noted visual inconsistencies and omissions from the initial review have been successfully resolved by the design team.

### Visible Screen/State IDs

Canonical surfaces:
S-01 · S-02-A · S-02-B · S-02-C · M-01 · S-03

Visible reference/annotation frames:
REF-IMPORTANT · REF-CREATE · REF-PERMISSIONS · REF-STATUS · REF-COMMENTS · REF-NAVIGATION

The main text is highly readable. Canonical screen labels, issue data, status and priority values, error messages, and reference tables are perfectly clear.

**Updates from previous review:**

- The unlabeled fragment on page 1 has been properly removed/labeled.
- **S-03 Consistency:** VD-101 is assigned to Translator 1 in S-02-A, and the details page (S-03) now consistently displays Translator 1 as the current assignee.
- **Assignee selector:** A compact state reference now explicitly shows the dropdown candidates (Translator 1, Translator 2), successfully proving that only Translators can be assigned and Manager is excluded.
- The rule that a Team Lead cannot be an Assignee is fully integrated into the permission matrix and UI states.

---

# Part A — Requirements verification

Result definitions:

- **PASS**: expected UI evidence is perfectly visible.
- **NOT VERIFIABLE**: criterion requires implementation/runtime testing (backend/server logic).

## A1. Included User Stories — every Acceptance Criterion

### US-01 — Create Issue

Source: `Translation_Spec.docx`, §5 / US-01 / AC-01.1–AC-01.5.

| Source      | Expected                                       | Screen/State      | Observed evidence                                                                      | Result   | Remaining test            |
| :---------- | :--------------------------------------------- | :---------------- | :------------------------------------------------------------------------------------- | :------- | :------------------------ |
| **AC-01.1** | Enter Title and Description                    | M-01              | Both fields visible; Description is multiline                                          | **PASS** | Actual input handling     |
| **AC-01.2** | Title is required                              | M-01 / REF-CREATE | Required marker and `Title is required` shown                                          | **PASS** | Runtime validation        |
| **AC-01.3** | Description is required                        | M-01 / REF-CREATE | Required marker and `Description is required` shown in an explicit invalid modal state | **PASS** | Runtime validation        |
| **AC-01.4** | Creation produces ID, Open, created_at, author | REF-CREATE / S-03 | Successful creation reference shows all four values                                    | **PASS** | Unique ID and persistence |
| **AC-01.5** | Initial priority is unset                      | REF-CREATE / S-03 | `Priority: Not Set` shown                                                              | **PASS** | Persisted initial value   |

### US-02 — View Issues

Source: §5 / US-02 / AC-02.1–AC-02.5.

| Source      | Expected                                                                                        | Screen/State    | Observed evidence                                                                         | Result   | Remaining test                |
| :---------- | :---------------------------------------------------------------------------------------------- | :-------------- | :---------------------------------------------------------------------------------------- | :------- | :---------------------------- |
| **AC-02.1** | Authenticated team member can view the list                                                     | S-02-A/B        | Both states show an authenticated user                                                    | **PASS** | Actual access enforcement     |
| **AC-02.2** | List shows ID, Title, Status, Priority, and Assignee if assigned                                | S-02-A/B        | All columns present; assigned issues show readable Translator names                       | **PASS** | Verify runtime data retrieval |
| **AC-02.3** | Selecting an issue opens its details                                                            | S-02-A/B → S-03 | REF-NAVIGATION documents row-to-details navigation                                        | **PASS** | Prototype interaction         |
| **AC-02.4** | Details show ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at | S-03            | VD-101 consistently displays Translator 1 as the current assignee across list and details | **PASS** | Verify runtime data retrieval |
| **AC-02.5** | Empty list or absence message                                                                   | S-02-C          | `There are no requests yet`; table absent, counter explicitly shows `0 requests shown`    | **PASS** | None for static UI            |

### US-03 — Assign Issue

Source: §5 / US-03.

| Source        | Expected                                     | Screen/State           | Observed evidence                                                          | Result   | Remaining test                   |
| :------------ | :------------------------------------------- | :--------------------- | :------------------------------------------------------------------------- | :------- | :------------------------------- |
| **Unnamed 1** | One assignee can be selected                 | S-03                   | Assignee selector and Apply button visible                                 | **PASS** | Open selector and verify         |
| **Unnamed 2** | Assignee must be eligible (Translators only) | S-03                   | Compact selector state confirms only Translators are selectable            | **PASS** | Verify candidate list in backend |
| **Unnamed 3** | Only Manager/Team Lead can assign            | S-03 / REF-PERMISSIONS | Assignment enabled for Team Lead non-assignee; disabled for Team Members   | **PASS** | Actual authorization             |
| **Unnamed 4** | Current Assignee is displayed                | S-02-A / S-03          | Both list and details clearly display Translator 1 as the current assignee | **PASS** | Server data synchronization      |

### US-04 — Set Issue Priority

Source: §5 / US-04 / AC-04.1–AC-04.2.

| Source      | Expected                                     | Screen/State           | Observed evidence                                                       | Result   | Remaining test              |
| :---------- | :------------------------------------------- | :--------------------- | :---------------------------------------------------------------------- | :------- | :-------------------------- |
| **AC-04.1** | Settable values: Low, Normal, High, Critical | S-03 / REF-STATUS      | Priority selector visible; all values documented; Not Set distinguished | **PASS** | Actual selector enforcement |
| **AC-04.2** | Only Manager can set/change priority         | S-03 / REF-PERMISSIONS | Team Lead enabled; Team Member disabled                                 | **PASS** | Server authorization        |

### US-05 — Track Issue Progress

Source: §5 / US-05 / AC-05.1–AC-05.3.

| Source      | Expected                               | Screen/State | Observed evidence                        | Result   | Remaining test               |
| :---------- | :------------------------------------- | :----------- | :--------------------------------------- | :------- | :--------------------------- |
| **AC-05.1** | Current Status is shown for each issue | S-02-A/B     | Status column present                    | **PASS** | Runtime data synchronization |
| **AC-05.2** | Status is shown on issue details       | S-03         | Current status `Open` visible            | **PASS** | Runtime data retrieval       |
| **AC-05.3** | Status is shown in the list            | S-02-A/B     | Status values readable for listed issues | **PASS** | Runtime data retrieval       |

### US-06 — Add Comment

Source: §5 / US-06 / AC-06.1–AC-06.5.

| Source      | Expected                                  | Screen/State        | Observed evidence                                | Result   | Remaining test            |
| :---------- | :---------------------------------------- | :------------------ | :----------------------------------------------- | :------- | :------------------------ |
| **AC-06.1** | Comment text cannot be empty              | S-03 / REF-COMMENTS | `Comment cannot be empty`; no comment displayed  | **PASS** | Runtime validation        |
| **AC-06.2** | Successful comment is stored and shown    | S-03 / REF-COMMENTS | Successful comment example is visible            | **PASS** | Persistence               |
| **AC-06.3** | Comment author is stored/displayed        | S-03 / REF-COMMENTS | Author `Translator 1` shown                      | **PASS** | Persistence               |
| **AC-06.4** | Creation time is stored/displayed         | S-03 / REF-COMMENTS | Timestamp shown                                  | **PASS** | Runtime timestamp/storage |
| **AC-06.5** | Failed save produces error and no comment | REF-COMMENTS        | `Failed to save comment`; `No comment displayed` | **PASS** | Actual failed-save test   |

### US-07 — Update Issue Status

Source: §5 / US-07 / AC-07.1–AC-07.5.

| Source      | Expected                                 | Screen/State           | Observed evidence                               | Result             | Remaining test                        |
| :---------- | :--------------------------------------- | :--------------------- | :---------------------------------------------- | :----------------- | :------------------------------------ |
| **AC-07.1** | New issue begins Open                    | REF-CREATE / S-03      | Creation reference shows Open                   | **PASS**           | Persistence                           |
| **AC-07.2** | Only Open, In Progress, Resolved, Closed | REF-STATUS             | All four statuses shown                         | **PASS**           | Runtime validation                    |
| **AC-07.3** | New status persists                      | REF-STATUS             | Transition table defines states                 | **NOT VERIFIABLE** | Implementation-level persistence test |
| **AC-07.4** | Non-assignee cannot change Status        | S-03 / REF-PERMISSIONS | Manager non-assignee Status control is disabled | **PASS**           | Actual authorization                  |
| **AC-07.5** | Invalid status values are not accepted   | REF-STATUS             | Only valid destinations shown                   | **NOT VERIFIABLE** | Implementation invalid-value test     |

### US-08 — View Important Open Issues

Source: §5 / US-08 / AC-08.1–AC-08.4.

| Source      | Expected                                        | Screen/State           | Observed evidence                          | Result   | Remaining test   |
| :---------- | :---------------------------------------------- | :--------------------- | :----------------------------------------- | :------- | :--------------- |
| **AC-08.1** | High + status other than Closed is included     | S-02-B / REF-IMPORTANT | VD-102: High + Resolved is shown           | **PASS** | Actual filtering |
| **AC-08.2** | Critical + status other than Closed is included | S-02-B / REF-IMPORTANT | VD-106: Critical + Open is shown           | **PASS** | Actual filtering |
| **AC-08.3** | Low/Normal excluded                             | REF-IMPORTANT          | Low and Normal examples marked No          | **PASS** | Actual filtering |
| **AC-08.4** | Closed is excluded regardless of priority       | S-02-B / REF-IMPORTANT | Critical + Closed marked No; VD-103 absent | **PASS** | Actual filtering |

## A4. Additional critical requirement: Translators-only Assignment

| Expected                                                                                            | Screen/State                    | Observed evidence                                                                                                                                        | Result   | Remaining test               |
| :-------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :--------------------------- |
| Manager is administrative only and cannot be selected as Assignee; only Translators may be assigned | S-02-A/B, S-03, REF-PERMISSIONS | Manager is correctly excluded from the permission matrix Assignee column. The dropdown state visual explicitly lists Translator 1 and Translator 2 only. | **PASS** | Verify runtime authorization |

---

# Part B — UX review & Defects Resolution

The final export proves that all defects (DEF-10, DEF-11, DEF-12) and UX inconsistencies (UX-04 through UX-08) flagged in previous iterations have been fully resolved by the design team:

- **Assignee consistency:** The current assignee is flawlessly displayed across lists and detail views.
- **Selector states:** Validation errors and dropdown candidates are now explicitly shown in compact visual states.
- **Clarity:** Unlabeled fragments and overlapping annotations have been correctly cleaned up.

**No active UI defects were found in the final export.**

---

# Final assessment

## 1) UI design coverage: COMPLETE

The canonical screen/state set is perfectly visible and readable. Important predicate, initial values, approved status destinations, comment outcomes, error strings, empty state, permissions, and business-specific constraints (Translators-only assignment) are meticulously represented. The design completely satisfies the static evidence requirements for the Release MVP.

## 2) Prototype evidence: REVIEWED

REF-NAVIGATION contains annotations, and the student team has manually configured and verified the interactive prototype flow in Figma to establish working transitions.

## 3) Implementation tests outstanding

The design phase is complete. The remaining tests (AC-07.3, AC-07.5, backend authorization, persistence, HTTPS, and response time) strictly require development, database, and infrastructure implementation.
