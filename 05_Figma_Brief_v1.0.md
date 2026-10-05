# VerbaDesk — Figma Design Implementation Brief

## 1. Purpose and scope

Create the **Release MVP Figma design** for VerbaDesk as a **desktop, grayscale, low-fidelity** internal web application. The design covers only the approved canonical surfaces **S-01, S-02-A/B/C, M-01, and S-03**, with issue assignment, priority, status, and comments represented as controls/states inside S-03 rather than separate screens.

**Release scope:** US-01–US-08. US-09, SR-10, and SR-11 are excluded from this design brief. [§8 / Release Scope]

**Blocker check: NONE.** All UX decisions required for the agreed screen architecture are covered by approved D-01–D-22.

---

## 2. Design constraints

- Desktop only.
- Grayscale.
- Low-fidelity.
- One working Figma page only.
- Use **editable Figma-native layers**, not flattened images.
- Use **reusable local components** for repeated controls/states.
- Use **Auto Layout where appropriate**.
- Use meaningful layer, frame, component, and variant names.
- Keep explanatory annotations **outside product frames**.
- Do not create separate screens for Assignment, Priority, Status, or Comments.
- Do not create full pages for every role × status combination; use compact state references/examples.
- Do not add page icon placeholders, decorative squares, row-selection controls, or bulk-selection/bulk-action controls.
- Do not introduce new fields, roles, authentication methods, product functions, or persistence mechanisms.
- S-01 is an **authentication boundary placeholder**, not a designed login screen.
- Use dummy data and predetermined prototype outcomes; do not implement real login, database, backend, conditional logic, or permission enforcement. [D-01], [D-02], [D-08]

**Source:** §1 / КС-01–КС-04, §5 / US-01–US-08.

---

## 3. Canonical screens and states

| ID         | Type                   | Name                    | Purpose                                                     |
| ---------- | ---------------------- | ----------------------- | ----------------------------------------------------------- |
| **S-01**   | Boundary / placeholder | Authentication boundary | Mark required authentication boundary; no login UI          |
| **S-02-A** | Screen state           | Issues List — All       | View all registered issues                                  |
| **S-02-B** | Screen state           | Issues List — Important | View important non-Closed issues                            |
| **S-02-C** | Screen state           | Issues List — Empty     | Show no-issues message                                      |
| **M-01**   | Modal / overlay        | Create Issue            | Create a new issue                                          |
| **S-03**   | Screen                 | Issue Details           | View issue details and perform approved issue-level actions |

**Source/D-ID:** [US-02/AC-02.1–02.5], [US-03], [US-04], [US-05], [US-06], [US-07], [US-08], [D-01], [D-02], [D-07]

---

## 4. Required labels and data

### S-01

Only the authentication boundary is represented.

Do **not** design:

- login form;
- credentials fields;
- login button;
- authentication method;
- authentication error screens.

**Source/D-ID:** [BR-01], [NFR-02], [D-02]

---

### S-02-A / S-02-B

Each displayed issue must expose at least:

| Data     | Requirement                    | Missing value          |
| -------- | ------------------------------ | ---------------------- |
| ID       | Required for an existing issue | Not expected           |
| Title    | Required                       | Not expected           |
| Status   | Required/current               | Not expected           |
| Priority | May be unset                   | **Not Set**            |
| Assignee | Conditional                    | Absent when unassigned |

The `All` and `Important` tabs are the canonical list states. [D-01]

**Source/D-ID:** [US-02/AC-02.2], [US-05/AC-05.3], [BR-04], [BR-05], [D-01], [D-03], [D-04]

---

### S-02-C

Show:

**`There are no requests yet`**

Do not use an empty table as the canonical empty state.

**Source/D-ID:** [US-02/AC-02.5], [D-11], [D-16]

---

### M-01

Only these are **user-entered fields**:

| Field           | Required | Content          |
| --------------- | -------- | ---------------- |
| **Title**       | Yes      | Text             |
| **Description** | Yes      | Any text or link |

No separate format validation for Description.

After successful creation, these values are automatic:

| Data       | Result                     |
| ---------- | -------------------------- |
| ID         | Unique ID                  |
| Status     | `Open`                     |
| Created at | Creation timestamp         |
| Author     | Current authenticated user |
| Priority   | `Not Set`                  |
| Assignee   | Absent                     |

**Source/D-ID:** [US-01/AC-01.1–01.5], [BR-02–BR-04], [D-03], [D-04], [D-09]

---

### S-03

Show at minimum:

- ID
- Title
- Description
- Status
- Priority
- Assignee, when assigned
- Author
- Created at
- Comments, when present

Priority `Not Set` is distinct from the settable values `Low`, `Normal`, `High`, `Critical`.

Date/time may be represented as `DD.MM.YYYY HH:MM`.

**Source/D-ID:** [US-02/AC-02.4], [US-04/AC-04.1], [US-06/AC-06.2–06.4], [BR-04–BR-05], [D-03], [D-14]

---

## 5. Controls and permissions

### Canonical control set

| C-ID     | Label       | Type                 | Enabled condition                     | Result                                     |
| -------- | ----------- | -------------------- | ------------------------------------- | ------------------------------------------ |
| **C-02** | All         | Tab                  | Authenticated user                    | S-02-A                                     |
| **C-03** | Important   | Tab                  | Authenticated user                    | S-02-B                                     |
| **C-05** | Create      | Button               | Authenticated team member             | Open M-01                                  |
| **C-06** | Title       | Text input           | M-01 open                             | Enter Title                                |
| **C-07** | Description | Multiline text input | M-01 open                             | Enter Description                          |
| **C-08** | Create      | Button               | M-01 open                             | Validate/create issue                      |
| **C-09** | Close modal | Button/control       | M-01 open                             | Return to previous S-02 state              |
| **C-10** | Assignee    | Selector             | Team Lead                             | Select one team member                     |
| **C-11** | Apply       | Button               | Team Lead                             | Apply assignee; stay S-03                  |
| **C-12** | Priority    | Selector             | Team Lead                             | Select Low/Normal/High/Critical            |
| **C-13** | Apply       | Button               | Team Lead                             | Apply priority; stay S-03                  |
| **C-14** | Status      | Selector             | Current Assignee, except Closed state | Select an allowed destination              |
| **C-15** | Apply       | Button               | Current Assignee, except Closed state | Apply allowed status transition; stay S-03 |
| **C-16** | Comment     | Multiline text input | Authenticated team member             | Enter comment                              |
| **C-17** | Add Comment | Button               | Authenticated team member             | Save/display comment or show error         |
| **C-18** | Back        | Button/control       | S-03                                  | Return to previous S-02 state              |

Widget types and Apply behavior are approved, not invented for this brief.

**Source/D-ID:** [US-03], [US-04/AC-04.1–04.2], [US-06], [US-07], [D-06], [D-15], [D-18], [D-19]

### Permission rule

Assignee is a **relationship to the current issue**, not a global role.

| Context                    | View    | Create  | Comment | Assign      | Change priority | Change status |
| -------------------------- | ------- | ------- | ------- | ----------- | --------------- | ------------- |
| Team Member — non-assignee | Allowed | Allowed | Allowed | Disabled    | Disabled        | Disabled      |
| Team Member — assignee     | Allowed | Allowed | Allowed | Disabled    | Disabled        | **Enabled**   |
| Team Lead — non-assignee   | Allowed | Allowed | Allowed | **Enabled** | **Enabled**     | Disabled      |
| Team Lead — assignee       | Allowed | Allowed | Allowed | **Enabled** | **Enabled**     | **Enabled**   |

Role-dependent forbidden controls remain **visible but disabled**; they are not hidden.

**Team Lead does not gain status permission automatically.** Status permission is based on current assignee relation.

**Source/D-ID:** [BR-02], [BR-06], [BR-08], [US-03], [US-04/AC-04.2], [US-07/AC-07.4], [D-05]

---

## 6. Status transitions

The canonical status values are:

`Open` · `In Progress` · `Resolved` · `Closed`

Only these transitions are allowed:

| From        | To          | Condition                                               |
| ----------- | ----------- | ------------------------------------------------------- |
| Open        | In Progress | Current Assignee                                        |
| In Progress | Resolved    | Current Assignee                                        |
| Resolved    | In Progress | Current Assignee; reopen work                           |
| Resolved    | Closed      | Current Assignee                                        |
| Open        | Closed      | Current Assignee; client cancellation before processing |

All other transitions are forbidden.

`Closed` has **no outgoing status transitions**.

The current status is a displayed state, not a transition.

Forbidden status destinations are **not shown in the selector**. In `Closed`, the status selector remains visible but is disabled.

**Source/D-ID:** [BR-07], [BR-08], [BR-08.1], [US-07/AC-07.2–07.5], [D-05], [D-06], [D-21]

---

## 7. Important predicate

An issue appears in **S-02-B Important** exactly when:

`priority ∈ {High, Critical} AND status != Closed`

Required compact reference examples:

| Priority | Status   | Important |
| -------- | -------- | --------- |
| High     | Resolved | Yes       |
| Critical | Closed   | No        |
| Not Set  | Open     | No        |
| Not Set  | Resolved | No        |

Do not substitute `status = Open` for `status != Closed`.

**Source/D-ID:** [BR-09], [BR-09.1], [US-08/AC-08.1–08.4], [D-10]

---

## 8. Required empty and error examples

The Figma work must contain compact examples of the following states.

### M-01 validation

**Title empty**

- Issue is not created.
- Show: **`Title is required`**
- Remain in M-01.

**Description empty**

- Issue is not created.
- Show: **`Description is required`**
- Remain in M-01.

**Description containing a link**

- Accepted without separate format validation.

**Source/D-ID:** [US-01/AC-01.2–01.3 + creation scenarios], [D-09], [D-17]

### S-02-C empty state

Show:

**`There are no requests yet`**

**Source/D-ID:** [US-02/AC-02.5], [D-11], [D-16]

### S-03 comment states

**Empty comment**

- No comment is created.
- Show: **`Comment cannot be empty`**

**Failed save**

- No comment is created.
- Show: **`Failed to save comment`**

**Success**

- Comment is stored/displayed.
- Show comment author and creation date/time.

**Source/D-ID:** [US-06/AC-06.1–06.5], [D-18], [D-20]

---

## 9. Navigation intent

The prototype at this stage should contain **annotations of intended transitions only**. Do not implement conditional logic, real authentication, backend behavior, or permission logic.

Canonical navigation:

`S-01 → S-02-A`

`S-02-A ↔ S-02-B`

`S-02-A / S-02-B / S-02-C → M-01`

`S-02-A / S-02-B → S-03`

`M-01 successful Create → S-03`

`M-01 close → previous S-02 state`

`S-03 Back → previous S-02 state`

For S-03 actions that successfully change assignee, priority, status, or comment, remain on **S-03**.

When S-03 was entered from `S-02-B`, Back returns to `S-02-B`; when entered from `S-02-A`, Back returns to `S-02-A`.

Forbidden actions do not create navigation transitions.

**Source/D-ID:** [US-02/AC-02.3], [US-07/AC-07.3], [D-06], [D-07], [D-19], [D-22]

---

## 10. State reference

Create **compact reference examples**, not complete duplicate pages.

### Required status reference

Show all four statuses somewhere in the S-03 state reference:

- Open
- In Progress
- Resolved
- Closed

The examples must demonstrate that:

- Resolved is distinct from Closed;
- Resolved can remain Important when priority is High/Critical;
- Closed has no outgoing status transition.

**Source/D-ID:** [BR-07], [BR-08.1], [BR-09.1], [US-08/AC-08.1–08.4], [D-10], [D-21]

### Required role × assignee reference

Show all four contexts in a compact permission/state reference:

1. Team Member — non-assignee
2. Team Member — assignee
3. Team Lead — non-assignee
4. Team Lead — assignee

For each context, show the relevant assignment, priority, and status controls in their approved enabled/disabled states.

Do **not** create four full S-03 pages.

**Source/D-ID:** [BR-06], [BR-08], [US-03], [US-04/AC-04.2], [US-07/AC-07.4], [D-05]

---

## 11. Sample data

Use **fictional/dummy data** only.

Recommended role-labeled users:

- Translator 1
- Translator 2
- Manager

These represent the three predefined team members from the source; names are demonstration data, not product requirements.

Recommended issue examples:

**Issue A — creation example**

- Title: `Translation of Contract`
- Description: `Translate to EN`
- Status: Open
- Priority: Not Set
- Assignee: absent
- Author: Translator 1

**Issue B — Important / Resolved**

- Status: Resolved
- Priority: High
- Assignee: Translator 1

**Issue C — excluded from Important**

- Status: Closed
- Priority: Critical

**Issue D — unset priority**

- Status: Open
- Priority: Not Set
- Assignee: absent

The Title/Description example comes from the source creation scenario; the remaining combinations are compact dummy states required to demonstrate the approved rules.

**Source/D-ID:** [US-01 creation scenario], [BR-01], [D-08], [D-10]

---

## 12. Exclusions

Do not add any of the following:

- Attachments / file-upload UI;
- AI categorization;
- AI priority recommendation;
- customer support portal;
- billing;
- time tracking;
- full project-management functionality;
- mobile application;
- external tracker/CRM integrations;
- issue deletion;
- user-facing change history;
- separate Assignment screen;
- separate Priority screen;
- separate Status screen;
- separate Comments screen;
- bulk selection/actions;
- page icon placeholders;
- decorative placeholder shapes.

**Source/D-ID:** [§2 / Out of Scope], [BR-10], [BR-11], [§8 / Release Scope], [D-01]

HTTPS, backup, response time, and actual authentication are **implementation concerns, not product frames**.

**Source:** [NFR-01–NFR-04] / §8.

---

## 13. Acceptance checklist

### Structure

- [ ] One working Figma page only.
- [ ] Editable Figma-native layers.
- [ ] Meaningful names.
- [ ] Reusable local components.
- [ ] Auto Layout used where appropriate.
- [ ] Grayscale, desktop, low-fidelity.
- [ ] Notes/annotations placed outside product frames.

### Canonical surfaces

- [ ] S-01 exists only as authentication boundary placeholder.
- [ ] S-02-A All exists.
- [ ] S-02-B Important exists.
- [ ] S-02-C Empty exists.
- [ ] M-01 Create modal exists.
- [ ] S-03 Issue Details exists.
- [ ] No separate Assignment/Priority/Status/Comments screens.

### Data and controls

- [ ] M-01 contains only Title and Description as user-entered fields.
- [ ] Title and Description are required.
- [ ] Description accepts text or link without format validation.
- [ ] Created issue reference shows Open, Not Set, absent Assignee, ID, Author, Created at.
- [ ] S-02 list shows ID, Title, Status, Priority, Assignee when assigned.
- [ ] S-03 shows required issue and comment data.
- [ ] Not Set is distinct from Low/Normal/High/Critical.
- [ ] Apply is present for assignee, priority, and status changes.
- [ ] Forbidden role-dependent controls are visible disabled.
- [ ] Team Lead non-assignee cannot change status.
- [ ] Team Lead assignee can change status.

### Status

- [ ] All four statuses appear in the state reference.
- [ ] Only BR-08.1 transitions are represented as allowed.
- [ ] Resolved → In Progress is represented.
- [ ] Open → Closed cancellation case is represented.
- [ ] Closed has no outgoing transition.
- [ ] Forbidden status destinations are absent from the selector.
- [ ] Closed selector is visible but disabled.

### Important

- [ ] Predicate is exactly `priority ∈ {High, Critical} AND status != Closed`.
- [ ] High + Resolved is included.
- [ ] Critical + Closed is excluded.
- [ ] Not Set examples are excluded.
- [ ] Low/Normal examples are excluded.

### Empty/error

- [ ] S-02-C says `There are no requests yet`.
- [ ] Empty Title shows `Title is required`.
- [ ] Empty Description shows `Description is required`.
- [ ] Empty Comment shows `Comment cannot be empty`.
- [ ] Failed comment save shows `Failed to save comment`.
- [ ] Failed comment save has no created comment.
- [ ] Successful comment shows author and creation date/time.

### Navigation

- [ ] S-01 → S-02-A is annotated.
- [ ] All ↔ Important is annotated.
- [ ] Create opens M-01.
- [ ] Successful Create → S-03 is annotated.
- [ ] M-01 close returns to previous S-02 state.
- [ ] S-03 Back returns to previous S-02 state.
- [ ] Successful assignment/priority/status/comment action remains on S-03.
- [ ] Forbidden actions have no transition.

### Prototype boundary

- [ ] Prototype contains transition annotations only at this stage.
- [ ] No real login.
- [ ] No backend/database implementation.
- [ ] No coded conditional logic.
- [ ] No coded permission enforcement.

**Verification note:** this checklist is an implementation acceptance checklist; it does not imply PASS before the Figma work and prototype are actually produced and reviewed.

---

## 14. Source / decision reference

**Primary source:** `Translation_Spec.docx`, §3 Business Rules, §5 User Stories + Acceptance Criteria, §6 NFR, §8 final Release Scope.

**Approved D-Decisions used:** D-01–D-22.

No Candidate MVP, discussion history, backend design, or Deferred/Future implementation detail is required by this brief.
