# VerbaDesk — Evidence-Based Review of Supplied Design Export

## Initial evidence check

**Reviewed artifact:** `VerbaDesk.pdf`, 13 pages.

### Visible Screen/State IDs

| Visible ID | Type                         |
| ---------- | ---------------------------- |
| **S-01**   | Authentication boundary      |
| **S-02-A** | Issues List — All            |
| **S-02-B** | Issues List — Important      |
| **S-02-C** | Issues List — Empty          |
| **M-01**   | Create Issue modal / overlay |
| **S-03**   | Issue Details                |

Additional visible reference/annotation pages are labeled:

`REF-IMPORTANT`, `REF-CREATE`, `REF-PERMISSIONS`, `REF-STATUS`, `REF-COMMENTS`, `REF-NAVIGATION`.

Page 1 contains an unlabeled partial component/state sample; I do **not** treat it as a canonical product screen.

### Readability

The exported text is generally readable. The canonical screen titles, labels, controls, status/priority values, validation messages, and reference annotations can be read.

One important readability limitation is visible in **S-02-A/S-02-B**: assigned rows display `...` in the Assignee column rather than a readable assignee name. Therefore the _presence_ of an assignee is not sufficiently observable there. The PDF itself confirms the list structure and fields, but the actual assignee identity is not readable.

I am reviewing the supplied PDF as the evidence artifact. I am **not** treating any Figma Agent report as proof of an implemented change.

---

# Part A — Requirements verification

## A1. Included User Stories — AC coverage

### US-01 — Create Issue

Source: §5 / US-01 / AC-01.1–AC-01.5.

| Source      | Expected                                     | Screen/State             | Observed evidence                                                                                          | Result   | Remaining test                                               |
| ----------- | -------------------------------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------ |
| **AC-01.1** | User can enter Title and Description         | M-01                     | Title and Description fields are visible; Description is multiline                                         | **PASS** | None for UI; actual input handling still implementation      |
| **AC-01.2** | Title required                               | M-01                     | `Title *` visible; REF-CREATE shows `Title empty → Title is required`                                      | **PASS** | Implementation validation                                    |
| **AC-01.3** | Description required                         | M-01                     | `Description *` visible; REF-CREATE shows `Description empty → Description is required`                    | **PASS** | Implementation validation                                    |
| **AC-01.4** | After creation: ID, Open, created_at, author | M-01 / S-03 / REF-CREATE | REF-CREATE explicitly shows unique ID, Open, Created at, Author; S-03 visibly shows the corresponding data | **PASS** | Actual creation/persistence and actual current-user identity |
| **AC-01.5** | Initial priority is not set                  | M-01 / S-03 / REF-CREATE | REF-CREATE shows `Priority Not Set`; S-03 shows `Not Set`                                                  | **PASS** | Actual persisted initial value                               |

The source creation scenario for empty Title is represented in REF-CREATE.

---

### US-02 — View Issues

Source: §5 / US-02 / AC-02.1–AC-02.5.

| Source      | Expected                                                                                        | Screen/State    | Observed evidence                                                                                              | Result                       | Remaining test                                                             |
| ----------- | ----------------------------------------------------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------- | -------------------------------------------------------------------------- |
| **AC-02.1** | Authenticated team member can view issue list                                                   | S-02-A/B        | Both list states are present and labeled; authenticated user is shown                                          | **PASS**                     | Actual access enforcement                                                  |
| **AC-02.2** | List shows ID, Title, Status, Priority, Assignee if assigned                                    | S-02-A/B        | All five columns exist. Assigned rows show `...`, so actual assignee identity is not readable                  | **PARTIAL**                  | Show readable assignee for an assigned issue; verify actual data retrieval |
| **AC-02.3** | Select an issue and open details                                                                | S-02-A/B → S-03 | Navigation annotation explicitly states rows open S-03                                                         | **PASS** for design evidence | Prototype interaction                                                      |
| **AC-02.4** | Details show ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at | S-03            | All required data areas are visible for VD-101; no assigned assignee because the displayed issue is unassigned | **PASS**                     | Actual data retrieval/persistence                                          |
| **AC-02.5** | Empty list can be empty or message                                                              | S-02-C          | Canonical empty state is shown with `There are no requests yet` and no table                                   | **PASS**                     | None for static UI                                                         |

S-02-C is explicitly marked as the canonical empty state and shows the required message.

---

### US-03 — Assign Issue

Source: §5 / US-03. The source criteria are **four unnamed bullets**, and no new AC IDs are introduced here.

| Source                  | Expected                       | Screen/State           | Observed evidence                                                                                                                 | Result                   | Remaining test                                                                           |
| ----------------------- | ------------------------------ | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ---------------------------------------------------------------------------------------- |
| US-03 / unnamed point 1 | One assignee can be selected   | S-03                   | `Assignee` selector + `Apply` visible                                                                                             | **PASS**                 | Actual selector restriction/persistence                                                  |
| US-03 / unnamed point 2 | Assignee must be a team member | S-03 / REF-PERMISSIONS | Team contexts are shown, but no actual selector options are visible                                                               | **PARTIAL**              | Implementation must enforce team-member-only assignee values                             |
| US-03 / unnamed point 3 | Only Manager can assign        | S-03 / REF-PERMISSIONS | Manager/non-assignee S-03 shows assignment enabled; permission reference shows Team Member disabled and Team Lead enabled         | **PASS** for UI evidence | Actual authorization enforcement                                                         |
| US-03 / unnamed point 4 | Current assignee displayed     | S-02-A/B / S-03        | S-03 can show the assignee field, but list assigned rows render `...`; current assigned identity is not readable in list evidence | **PARTIAL**              | Show readable assigned name in at least an assigned list state; implementation data test |

---

### US-04 — Set Issue Priority

Source: §5 / US-04 / AC-04.1–AC-04.2.

| Source      | Expected                                        | Screen/State           | Observed evidence                                                                                        | Result                   | Remaining test                |
| ----------- | ----------------------------------------------- | ---------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------ | ----------------------------- |
| **АС-04.1** | Allowed set values: Low, Normal, High, Critical | S-03 / REF-STATUS      | Priority values are explicitly listed as `Not Set · Low · Normal · High · Critical`; S-03 shows selector | **PASS**                 | Actual validation/enforcement |
| **АС-04.2** | Only Manager may set/change priority            | S-03 / REF-PERMISSIONS | Team Member contexts disabled; Team Lead contexts enabled                                                | **PASS** for UI evidence | Server-side authorization     |

The source distinguishes `Not Set` from the installed priority values.

---

### US-05 — Track Issue Progress

Source: §5 / US-05 / AC-05.1–AC-05.3.

| Source      | Expected                            | Screen/State | Observed evidence                                                       | Result   | Remaining test              |
| ----------- | ----------------------------------- | ------------ | ----------------------------------------------------------------------- | -------- | --------------------------- |
| **АС-05.1** | Current status shown for each issue | S-02-A/B     | Status column visible with Open, Resolved, Closed, In Progress examples | **PASS** | Actual data synchronization |
| **AC-05.2** | Status shown on issue page          | S-03         | `Open` visible in issue details                                         | **PASS** | Actual retrieval            |
| **AC-05.3** | Status shown in list                | S-02-A/B     | Status column clearly visible                                           | **PASS** | Actual retrieval            |

---

### US-06 — Add Comment

Source: §5 / US-06 / AC-06.1–AC-06.5.

| Source      | Expected                                   | Screen/State        | Observed evidence                                                    | Result                   | Remaining test                       |
| ----------- | ------------------------------------------ | ------------------- | -------------------------------------------------------------------- | ------------------------ | ------------------------------------ |
| **AC-06.1** | Comment text cannot be empty               | S-03 / REF-COMMENTS | `Comment cannot be empty`; no comment displayed                      | **PASS**                 | Actual validation                    |
| **AC-06.2** | Added comment is stored and shown          | S-03 / REF-COMMENTS | Existing comment is shown; success reference shows displayed comment | **PASS** for UI evidence | Persistence                          |
| **AC-06.3** | Comment author stored/displayed            | S-03 / REF-COMMENTS | `Translator 1` displayed with comment                                | **PASS**                 | Actual persistence                   |
| **AC-06.4** | Creation date/time stored/displayed        | S-03 / REF-COMMENTS | `05.10.2026 10:42` visible                                           | **PASS**                 | Actual persistence/time source       |
| **AC-06.5** | Failed save → error and no comment created | REF-COMMENTS        | `Failed to save comment` + `No comment displayed` explicitly shown   | **PASS** for UI evidence | Actual failure injection/persistence |

The PDF explicitly states that validation and failed saves do not create a comment.

---

### US-07 — Update Issue Status

Source: §5 / US-07 / AC-07.1–AC-07.5.

| Source      | Expected                                                | Screen/State           | Observed evidence                                                                                                  | Result                   | Remaining test                                                                   |
| ----------- | ------------------------------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------ | -------------------------------------------------------------------------------- |
| **АС-07.1** | New issue starts Open                                   | S-03 / REF-CREATE      | Successful creation reference shows `Status Open`; S-03 example is Open                                            | **PASS**                 | Actual persistence                                                               |
| **АС-07.2** | Allowed statuses: Open, In Progress, Resolved, Closed   | REF-STATUS / S-03      | All four statuses visible; allowed destinations explicitly listed                                                  | **PASS**                 | Actual enum enforcement                                                          |
| **АС-07.3** | New status is stored and appears on next viewing        | S-03 / REF-STATUS      | Status changes are represented, but no evidence shows leaving/reopening the issue and verifying persistence        | **NOT VERIFIABLE**       | Implementation test: change → leave S-03 → reopen same issue → verify new status |
| **АС-07.4** | Non-assignee cannot change status                       | S-03 / REF-PERMISSIONS | Team Lead non-assignee S-03 has status visibly disabled; matrix also shows non-assignee disabled                   | **PASS** for UI evidence | Server-side authorization                                                        |
| **АС-07.5** | Invalid status value outside defined set is not allowed | REF-STATUS             | Only allowed destinations are displayed, but no test evidence demonstrates rejection of an invalid persisted value | **NOT VERIFIABLE**       | Implementation-level invalid-value test                                          |

---

### US-08 — View Important Open Issues

Source: §5 / US-08 / AC-08.1–AC-08.4.

| Source      | Expected                               | Screen/State           | Observed evidence                                                                   | Result   | Remaining test   |
| ----------- | -------------------------------------- | ---------------------- | ----------------------------------------------------------------------------------- | -------- | ---------------- |
| **AC-08.1** | High + non-Closed appears              | S-02-B / REF-IMPORTANT | VD-102 = High + Resolved and is shown                                               | **PASS** | Actual predicate |
| **AC-08.2** | Critical + non-Closed appears          | S-02-B / REF-IMPORTANT | VD-106 = Critical + Open and is shown                                               | **PASS** | Actual predicate |
| **AC-08.3** | Low/Normal excluded                    | S-02-B / REF-IMPORTANT | Reference table states Low/Normal examples = No; S-02-B contains only matching rows | **PASS** | Actual predicate |
| **AC-08.4** | Closed excluded regardless of priority | S-02-B / REF-IMPORTANT | Critical + Closed = No; S-02-B does not show VD-103                                 | **PASS** | Actual predicate |

The PDF explicitly shows `High + Resolved = Yes` and `Critical + Closed = No`.

---

# A2. UI-relevant Business Rules

| Source      | Expected                                                                  | Screen/State               | Observed evidence                                                                                   | Result                   | Remaining test                                  |
| ----------- | ------------------------------------------------------------------------- | -------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------ | ----------------------------------------------- |
| **BR-01**   | Only authenticated team members can use system; 2 Translators + 1 Manager | S-01 / S-02                | S-01 is explicitly an authentication boundary; S-02 shows authenticated Translator 1 / S-03 Manager | **PARTIAL**              | Actual authentication/access enforcement        |
| **BR-02**   | Any authenticated team member can create                                  | M-01 / S-02                | Create is shown for authenticated Translator; no Manager create example is present                  | **PARTIAL**              | Verify both permitted actors in implementation  |
| **BR-03**   | Initial status = Open                                                     | M-01 / REF-CREATE          | Successful creation reference shows Open                                                            | **PASS**                 | Actual persistence                              |
| **BR-04**   | Initial priority = Not Set                                                | M-01 / S-03 / REF-CREATE   | `Not Set` explicitly shown                                                                          | **PASS**                 | Actual persistence                              |
| **BR-05**   | Settable values Low/Normal/High/Critical; Not Set distinct                | REF-STATUS / S-03          | All values explicitly listed; S-03 distinguishes Not Set                                            | **PASS**                 | Actual enforcement                              |
| **BR-06**   | Only Manager changes priority                                             | S-03 / REF-PERMISSIONS     | Permission matrix and S-03 show Team Lead enabled, Team Member disabled                             | **PASS** UI              | Server authorization                            |
| **BR-07**   | Status only Open/In Progress/Resolved/Closed                              | REF-STATUS                 | All four shown                                                                                      | **PASS**                 | Actual implementation validation                |
| **BR-08**   | Status change belongs to responsible member                               | S-03 / REF-PERMISSIONS     | Team Lead non-assignee status disabled; matrix says status requires Current Assignee                | **PASS** UI              | Server authorization                            |
| **BR-08.1** | Only source-defined transitions allowed                                   | REF-STATUS                 | Allowed destinations shown for Open, In Progress, Resolved; Closed = None                           | **PASS**                 | Implementation enforcement                      |
| **BR-09**   | Important = High or Critical                                              | S-02-B / REF-IMPORTANT     | Predicate explicitly shown                                                                          | **PASS**                 | Actual filtering                                |
| **BR-09.1** | Resolved remains distinct from Closed and can remain Important            | REF-IMPORTANT / REF-STATUS | High + Resolved = Yes; explicit statement that Resolved remains distinct                            | **PASS**                 | Actual filtering/state implementation           |
| **BR-10**   | Issues cannot be deleted                                                  | S-02 / S-03                | No delete control visible anywhere in supplied canonical frames                                     | **PASS** for UI evidence | Backend enforcement if deletion endpoint exists |
| **BR-11**   | No separate user-facing change history                                    | S-03                       | No change-history UI visible                                                                        | **PASS** for UI evidence | Implementation cannot be inferred from mockup   |

BR-01/02 and the security boundaries remain implementation concerns beyond what a static export can prove. The source defines those rules explicitly.

---

# A3. Approved D-decisions

| Source/D-ID | Expected                                                                    | Screen/State                     | Observed evidence                                                                                                                        | Result                       | Remaining test                                  |
| ----------- | --------------------------------------------------------------------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ----------------------------------------------- |
| **D-01**    | Desktop, grayscale, low-fi; All/Important tabs; Create modal                | All                              | Export visibly follows desktop grayscale low-fi structure; tabs and modal exist                                                          | **PASS**                     | None for static design                          |
| **D-02**    | S-01 is auth boundary only; start authenticated at S-02                     | S-01/S-02                        | S-01 contains no login form; S-02 shows authenticated user                                                                               | **PASS** UI                  | Actual authentication                           |
| **D-03**    | Not Set visible; only four settable priorities; no reset                    | S-03 / REF-STATUS                | Not Set shown and settable values listed; no reset control visible                                                                       | **PASS**                     | Actual selector enforcement                     |
| **D-04**    | Initial assignee absent; Manager can reassign                               | M-01 / S-03 / REF-CREATE         | REF-CREATE says Assignee Absent; S-03 has assignee selector                                                                              | **PASS**                     | Actual persistence/reassignment                 |
| **D-05**    | Forbidden role-dependent controls visible disabled                          | S-03 / REF-PERMISSIONS           | Status disabled in Manager non-assignee screen; matrix shows disabled states                                                             | **PASS**                     | Server authorization                            |
| **D-06**    | Apply beside assignee/priority/status; success remains S-03                 | S-03 / REF-NAVIGATION            | Apply controls visible; navigation reference says successful actions remain on S-03                                                      | **PASS** for design evidence | Prototype/implementation                        |
| **D-07**    | Back preserves previous list state                                          | S-03 / REF-NAVIGATION            | S-03 explicitly states previous tab preserved; navigation annotation confirms                                                            | **PASS**                     | Prototype interaction                           |
| **D-08**    | Dummy data; predetermined prototype results; no real login/DB               | All / REF-NAVIGATION             | Dummy data is visible; annotation explicitly says no prototype interactions/backend behavior                                             | **PARTIAL**                  | Prototype evidence is not provided              |
| **D-09**    | Description accepts text/link without format validation                     | M-01 / REF-CREATE                | Description says `Enter any text or link`; URL example = Accepted                                                                        | **PASS**                     | Actual implementation                           |
| **D-10**    | Open Issues means status ≠ Closed, including Resolved                       | S-02-B / REF-IMPORTANT           | Resolved + High is shown; predicate explicitly says status != Closed                                                                     | **PASS**                     | Actual filter                                   |
| **D-11**    | Empty state uses message, not empty table                                   | S-02-C                           | Table absent; exact required message visible                                                                                             | **PASS**                     | None for UI                                     |
| **D-12**    | Open → Closed cancellation performed by Current Assignee                    | REF-STATUS                       | Open → Closed noted; all transitions require Current Assignee                                                                            | **PASS** for design evidence | Implementation authorization                    |
| **D-13**    | No attachment UI because US-09 deferred                                     | All canonical screens            | No attachment control/interface visible                                                                                                  | **PASS**                     | None for UI                                     |
| **D-14**    | Standard date/time format                                                   | S-03 / REF-CREATE / REF-COMMENTS | `05.10.2026 09:15`, `05.10.2026 10:42`, etc. visibly used                                                                                | **PASS**                     | Implementation timezone/storage semantics       |
| **D-15**    | Title text input; Description/Comment multiline; selectors; actions buttons | M-01 / S-03                      | Title = text field; Description = multiline; Assignee/Priority/Status = selectors; actions = buttons. **Comment is visibly single-line** | **FAIL**                     | Change Comment control to multiline             |
| **D-16**    | Empty-state copy exactly `There are no requests yet`                        | S-02-C                           | Exact phrase visible                                                                                                                     | **PASS**                     | None                                            |
| **D-17**    | Description error and stay on M-01                                          | M-01 / REF-CREATE                | Error `Description is required` visibly documented                                                                                       | **PASS** UI evidence         | Prototype/implementation stay-on-modal behavior |
| **D-18**    | Comment must be multiline                                                   | S-03 / REF-COMMENTS              | Comment input on S-03 is visually a one-line field                                                                                       | **FAIL**                     | Make Comment a multiline text input             |
| **D-19**    | Back/Close return previous S-02 state                                       | M-01 / S-03 / REF-NAVIGATION     | Explicit navigation annotations                                                                                                          | **PASS** design intent       | Prototype interaction                           |
| **D-20**    | Exact comment errors                                                        | REF-COMMENTS                     | Both exact strings visible                                                                                                               | **PASS**                     | Actual error injection                          |
| **D-21**    | Forbidden status destinations absent; Closed selector visible but disabled  | REF-STATUS / S-03                | Allowed destinations only shown; Closed = None + Disabled                                                                                | **PASS** UI evidence         | Actual enforcement                              |
| **D-22**    | Successful Create automatically M-01 → S-03                                 | REF-NAVIGATION                   | Explicit annotation: successful Create opens newly created issue                                                                         | **PASS** design intent       | Actual prototype/implementation navigation      |

The approved status and Important rules are visible in the dedicated reference states.

---

# A4. Specific required checks

## Permissions

The supplied permission reference correctly shows:

| Context                    | Assignment | Priority | Status   |
| -------------------------- | ---------- | -------- | -------- |
| Team Member — non-assignee | Disabled   | Disabled | Disabled |
| Team Member — assignee     | Disabled   | Disabled | Enabled  |
| Team Lead — non-assignee   | Enabled    | Enabled  | Disabled |
| Team Lead — assignee       | Enabled    | Enabled  | Enabled  |

This matches the approved rule that **status rights follow Current Assignee**, not Team Lead role.

**Result: PASS for visible permission-state evidence.**

It does **not** prove backend/server authorization.

---

## Current Assignee

D-04 is correctly represented for the newly created issue: assignee is absent. REF-CREATE explicitly states `Assignee Absent`.

**Result: PASS.**

For already assigned issues in the lists, however, the visible `...` means the actual assignee identity is not readable.

**Result for AC-02.2/US-03 current-assignee display: PARTIAL.**

---

## Initial values

- Status = `Open`
- Priority = `Not Set`
- Assignee = absent

All three are visibly represented in REF-CREATE.

**Result: PASS for UI evidence.**

---

## Status transitions

The source actually defines **five allowed transitions**, not four:

1. Open → In Progress
2. In Progress → Resolved
3. Resolved → Closed
4. Resolved → In Progress
5. Open → Closed for cancellation before processing

The supplied REF-STATUS shows these all. Closed has `None` as allowed destinations and its selector is disabled.

**Result: PASS for design evidence.**

No illegal transition is visibly presented as an allowed destination.

---

## Resolved ≠ Closed

The supplied reference explicitly states that Resolved is distinct from Closed and shows **High + Resolved = Important**.

**Result: PASS.**

---

## Important predicate

The exact displayed predicate is:

`priority ∈ {High, Critical} AND status != Closed`

The required examples are also visible:

- High + Resolved → Yes
- Critical + Closed → No
- Not Set + Open → No
- Not Set + Resolved → No

**Result: PASS.**

---

## Comments

The export contains all three required reference states:

- empty → `Comment cannot be empty` + no comment;
- failed save → `Failed to save comment` + no comment;
- success → displayed comment + author + timestamp.

**Result: PASS for visible UI evidence.**

Persistence remains implementation-only.

---

## Validation and empty states

Visible:

- `Title is required`
- `Description is required`
- `There are no requests yet`
- `Comment cannot be empty`
- `Failed to save comment`

**Result: PASS for visible reference evidence.**

S-02-C also contains an additional sentence, `No requests are available in this view.` above the canonical empty-state copy. This is not a requirements failure, but it is redundant with the required message.

---

## Extra functions / page icon placeholders

No delete, bulk selection, bulk action, attachment, AI, or separate Assignment/Priority/Status/Comment product screens are visible.

No page-icon placeholder controls or decorative icon blocks were observed in the canonical product frames.

**Result: PASS for visible evidence.**

This does not prove that no hidden interaction exists outside the supplied export.

---

## NFRs

The following must **not** receive PASS from the PDF:

| Requirement                        | Result             | Why                                            |
| ---------------------------------- | ------------------ | ---------------------------------------------- |
| **NFR-01 Response time < 1 s**     | **NOT VERIFIABLE** | Static export cannot measure runtime           |
| **NFR-02 Authentication required** | **NOT VERIFIABLE** | Boundary is visible, actual enforcement is not |
| **NFR-03 HTTPS only**              | **NOT VERIFIABLE** | Not a UI property proven by the export         |
| **NFR-04 Backup of all data**      | **NOT VERIFIABLE** | Not a UI property                              |

Final source NFRs are defined in §8.

---

# Part B — UX evidence review

## UX-01 — Comment input contradicts the approved multiline decision

**Area:** S-03, Comment control.

The visible S-03 comment input is a one-line field (`Write a comment`) despite D-18 requiring a **multiline text input**.

**Impact:** interaction affordance is inconsistent with the approved component contract and may constrain longer comments.

**Severity:** High.

This is a direct design defect, not merely a preference.

---

## UX-02 — Assignee information is not readable in the issue lists

**Area:** S-02-A and S-02-B, Assignee column.

Assigned rows visibly contain `...` rather than a readable assignee name.

**Impact:** users cannot reliably identify who is responsible directly from the list, weakening the purpose of assignment visibility and making AC-02.2/US-03 evidence incomplete.

**Severity:** Medium.

---

## UX-03 — S-02-C contains redundant empty-state messaging

**Area:** S-02-C.

The frame says `No requests are available in this view.` and separately centers the canonical `There are no requests yet`.

**Impact:** the two statements communicate essentially the same state and create unnecessary duplication around the required empty-state copy.

**Severity:** Low.

This is a clarity/consistency issue, not a new functional requirement.

---

## UX-04 — State/debug annotations appear inside the S-03 product content

**Area:** S-03.

Texts such as:

- `Enabled for Team Lead`
- `Not Set is a distinct value`
- `Visible, disabled: status requires Current Assignee`

appear inside the product detail/update area rather than as external annotations.

**Impact:** they can be read as product-facing UI copy rather than documentation/state annotation. The approved brief requested annotations outside product frames.

**Severity:** Medium.

---

## UX-05 — Disabled status selector displays destination values rather than a clear current-state representation

**Area:** S-03, Team Lead non-assignee state.

The issue's actual status is `Open`, while the disabled status selector visibly contains `In Progress / Closed`.

**Impact:** the viewer may interpret those values as the issue's current status rather than as possible destinations. The source distinguishes current status from transitions.

**Severity:** Medium.

This is a clarity issue in the evidence presentation; it does not change the underlying BR-08.1 rules.

---

# Part C — Defects and UX issues requiring human correction

## Requirement / contract defects

| DEF-ID     | Severity | Location                 | Source                                     | Evidence                                                                         | Minimal correction                                                                |
| ---------- | -------- | ------------------------ | ------------------------------------------ | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **DEF-08** | High     | S-03 Comment input       | **D-18 / D-15**                            | Comment field is visibly single-line, while approved decision requires multiline | Change only the Comment control to a multiline text input                         |
| **DEF-09** | Medium   | S-02-A/B Assignee column | **US-02 / AC-02.2; US-03 unnamed point 4** | Assigned rows show `...`; assignee identity is not readable                      | Display the actual dummy assignee name for assigned issues; no new field/function |

### Verification gaps, not UI defects

| ID         | Severity | Location                         | Source                            | Evidence                                                                                           | Minimal correction                                                                   |
| ---------- | -------- | -------------------------------- | --------------------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **VER-01** | High     | Status persistence               | **AC-07.3**                       | Export shows status states but no next-view persistence evidence                                   | Run implementation test: change status → leave issue → reopen → verify stored status |
| **VER-02** | High     | Invalid status value enforcement | **AC-07.5**                       | Allowed values are shown, but implementation rejection of an out-of-list value is not observed     | Run implementation-level invalid-status-value test                                   |
| **VER-03** | High     | Prototype navigation             | **D-06, D-07, D-19, D-22**        | REF-NAVIGATION contains annotations only and explicitly says no prototype interactions are defined | Provide/observe the actual prototype interaction run                                 |
| **VER-04** | High     | Security/NFR                     | **BR-01, NFR-02, NFR-03, NFR-04** | Static design cannot establish actual auth, HTTPS, backup, or access enforcement                   | Verify during implementation/infrastructure testing                                  |

The export itself explicitly states that no prototype interactions or backend behavior are defined.

---

## UX-only issues

These do **not** represent new product requirements.

| UX-ID     | Severity | Location    | Source             | Evidence                                                                            | Minimal correction                                                                                  |
| --------- | -------- | ----------- | ------------------ | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **UX-01** | Low      | S-02-C      | **D-16**           | Two overlapping empty-state messages                                                | Keep one clear empty-state presentation centered on the approved copy                               |
| **UX-02** | Medium   | S-03        | **D-01 / brief**   | State/debug notes embedded in content                                               | Move explanatory annotations outside the product frame                                              |
| **UX-03** | Medium   | S-03 Status | **BR-08.1 / D-21** | Disabled selector visually shows `In Progress / Closed` while current state is Open | Present current status and allowed destinations unambiguously without changing the transition rules |

The first two are presentation issues; they should not be interpreted as requests for new functionality.

---

# Final assessment

## 1) UI design coverage

**PARTIAL**

The canonical screen/state set is present:

**S-01, S-02-A, S-02-B, S-02-C, M-01, S-03.**

Most included ACs and UI-relevant rules have visible evidence. However, the supplied design has a direct approved-contract violation (**D-18: Comment must be multiline**) and incomplete visible assignee evidence in the list. Therefore this is not “complete” coverage.

---

## 2) Prototype evidence

**NOT PROVIDED**

The supplied PDF contains navigation annotations, but it explicitly says:

`No prototype interactions or backend behavior are defined.`

Therefore no prototype transition result has been independently verified.

Any claimed manual prototype results not directly observed here should be treated as **student-reported**, not as independently verified evidence.

---

## 3) Implementation tests outstanding

At minimum:

- **AC-07.3:** verify status persistence after leaving and reopening the issue.
- **AC-07.5:** verify an invalid status value outside the defined set cannot be persisted.
- Actual server authorization for assignment.
- Actual server authorization for priority.
- Actual assignee-based authorization for status, including **Team Lead non-assignee**.
- Actual issue persistence after Create.
- Actual comment persistence.
- Actual failed-comment-save behavior.
- Actual Important predicate/filtering.
- Actual authentication enforcement.
- HTTPS verification.
- Backup verification.
- Response-time verification.

**No implementation test result is marked PASS by this review merely because the corresponding UI is drawn.**
