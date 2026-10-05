# VerbaDesk — Developer-Verifiable Screen Contracts v1.0

## 0. Contract notation

**Verification levels:**

- **M — Mockup:** перевіряється статична структура, видимі дані, наявність/відсутність control, disabled/empty/validation presentation.
- **P — Prototype:** перевіряються interaction trigger, transition, state change та повернення.
- **I — Implementation:** перевіряються persistence, authorization enforcement, фактична валідація, фактичне збереження, backend/business-rule enforcement, NFR.
- **P/I** — interaction можна показати прототипом, але фактичний результат додатково потребує реалізації.

**Важливо:** наведені тести є **планами перевірки**, а не результатами. Для майбутнього макета/прототипу статус **PASS не встановлюється**.

---

# 1. S-01 — Authentication boundary

## 1.1 Contract

| Field | Contract |
|---|---|
| **ID** | S-01 |
| **Purpose** | Позначити обов’язкову authentication boundary перед доступом до MVP |
| **Actors** | Translator / Manager, authenticated team member |
| **Entry conditions** | Доступ до системи потребує authentication; демонстрація починається вже після authentication |
| **Visible data** | Authentication boundary placeholder; конкретний login UI не визначається |
| **Inputs** | Немає погоджених UI inputs |
| **Actions** | Немає погоджених login actions |
| **Role/state conditions** | Лише authenticated team members |
| **Errors / empty** | Не визначені для screen contract |
| **Exit** | T-01 → S-02-A |

**Source/D-ID:** §3 / BR-01; §6 / NFR-02; **D-02, D-08**.

## 1.2 Controls

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | Login UI не проєктується | **D-02** |

**Нова UX-деталізація не додається.**

## 1.3 Verification

**M:** перевіряється лише наявність placeholder boundary; не перевіряється справжня authentication.

**P:** демонстрація може стартувати на S-02 без login interaction.

**I:** перевіряється фактичне виконання NFR-02 та BR-01.

---

# 2. S-02-A — Issues List / All

## 2.1 Contract

| Field | Contract |
|---|---|
| **ID** | S-02-A |
| **Purpose** | Перегляд усіх зареєстрованих issues |
| **Actors** | Team Member, Team Lead |
| **Entry conditions** | Authenticated user; S-01 passed; або повернення з S-03 до попереднього All state |
| **Visible data** | Для кожної issue: ID, Title, Status, Priority, Assignee якщо призначений |
| **List state** | Tab = `All` |
| **Empty condition** | Якщо відповідний список не містить issues → S-02-C |
| **Exit** | M-01, S-03 або S-02-B |

**Source/D-ID:** US-02 / AC-02.1–AC-02.3; US-05 / AC-05.1, AC-05.3; **D-01, D-07, D-11**.

## 2.2 Visible data

| Field | Requiredness | Missing-value appearance |
|---|---|---|
| **ID** | Must exist after issue creation | Не має бути відсутнім для зареєстрованої issue |
| **Title** | Required at creation | Для валідної створеної issue не очікується відсутність |
| **Status** | System-assigned / always present | Не має бути відсутнім |
| **Priority** | Optional until set | **Not Set** |
| **Assignee** | Conditional | Якщо не призначений — assignee не має значення/відображається як відсутній; точний placeholder не заданий |
| **Created at** | Required after creation, але не мінімально обов’язковий у list за AC-02.2 | Не визначено для list |

**Priority:** відсутність = **Not Set**, а доступні встановлені значення тільки `Low / Normal / High / Critical`. **Source:** BR-04, BR-05; **D-03**.

## 2.3 Controls

| C-ID | Label | Type | Visible conditions | Enabled conditions | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-02 | All | Tab | Always | Always | Click | S-02-A | D-01 |
| C-03 | Important | Tab | Always | Always | Click | S-02-B | D-01, D-10 |
| C-04 | Select issue | Row / selectable issue | One per list item | If issue exists | Select | S-03 | US-02 / AC-02.3 |
| C-05 | Create | Button | Always for authenticated team member | Enabled for authenticated team member | Click | M-01 | BR-02; D-01 |

**Type details above are UX implementation choices where the source names the interaction but not the exact widget form.**  
**PROPOSED D-15:** use standard `Tab`, issue row selection and `Button` controls as shown above. Це не є нова product requirement.

## 2.4 States

**Default:** authenticated entry → S-02-A.  
**Empty:** → S-02-C, not blank table, per D-11.  
**Validation:** не застосовується до list.  
**Error:** конкретний loading/data-fetch error UI не визначений джерелом.

## 2.5 Transitions

**Incoming:** T-01, T-18.  
**Outgoing:** T-02, T-04, T-08, T-19.

---

# 3. S-02-B — Issues List / Important

## 3.1 Contract

| Field | Contract |
|---|---|
| **ID** | S-02-B |
| **Purpose** | Показ важливих незакритих issues |
| **Actors** | Team Member, Team Lead |
| **Entry conditions** | S-02 with `Important` selected |
| **Visible data** | ID, Title, Status, Priority, Assignee if assigned |
| **Predicate** | `priority ∈ {High, Critical} AND status != Closed` |
| **Exit** | S-02-A, S-03 |

**Source:** US-08 / AC-08.1–AC-08.4; BR-09, BR-09.1; **D-01, D-10**.

## 3.2 Visible data

| Field | Requiredness | Missing-value appearance |
|---|---|---|
| ID | Required for listed issue | None expected |
| Title | Required at creation | None expected for valid issue |
| Status | Required | None expected |
| Priority | Must be High/Critical for inclusion | Never `Not Set` for a matching issue |
| Assignee | Conditional | Absent if not assigned |

## 3.3 Controls

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-02 | All | Tab | Always | Always | Click | S-02-A | D-01 |
| C-03 | Important | Tab | Always | Always | Click | S-02-B | D-01 |
| C-04 | Select issue | Selectable row | When matching issue exists | Yes | Select | S-03 | US-02 / AC-02.3 |

## 3.4 Examples of predicate

| Priority | Status | Important |
|---|---|---|
| High | Resolved | **Yes** |
| Critical | Closed | **No** |
| Not Set | Open | **No** |
| Not Set | Resolved | **No** |
| Low | Open | **No** |
| Normal | In Progress | **No** |

**Source:** AC-08.1–AC-08.4 + D-10.

## 3.5 States

**Default:** selected Important tab.  
**Empty:** S-02-C if no matching issue.  
**Validation:** none.  
**Error:** specific retrieval error UI not specified.

## 3.6 Transitions

**Incoming:** T-02, T-18 from preserved Important state.  
**Outgoing:** T-03, T-08, T-19.

---

# 4. S-02-C — Issues List / Empty

## 4.1 Contract

| Field | Contract |
|---|---|
| **ID** | S-02-C |
| **Purpose** | Явно повідомити, що в поточному list context немає issues |
| **Actors** | Team Member, Team Lead |
| **Entry conditions** | S-02-A or S-02-B with zero applicable items |
| **Visible data** | Повідомлення про відсутність issues |
| **Inputs** | All / Important tab; Create |
| **Actions** | Switch list state; Create |
| **Exit** | S-02-A, S-02-B, M-01 |

**Source:** US-02 / AC-02.5; **D-11**.

## 4.2 Visible data

| Field | Requiredness | Missing appearance |
|---|---|---|
| Issue rows | None exist | No issue table/rows |
| Empty message | Required in this state | Message must be visible |
| Exact copy | Not specified | **UNRESOLVED / PROPOSED D-16 topic** |

## 4.3 Controls

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-02 | All | Tab | Yes | Yes | Click | S-02-A | D-01 |
| C-03 | Important | Tab | Yes | Yes | Click | S-02-B | D-01 |
| C-05 | Create | Button | Yes | Yes | Click | M-01 | BR-02; D-01 |

**PROPOSED D-16:** погодити точний empty-state copy. Не є product requirement.

## 4.4 States

**Default:** empty state.  
**Validation:** none.  
**Error:** not applicable; this is not a system failure state.

---

# 5. M-01 — Create Issue modal

## 5.1 Contract

| Field | Contract |
|---|---|
| **ID** | M-01 |
| **Purpose** | Створити нову translation issue |
| **Actors** | Translator, Manager |
| **Entry conditions** | Authenticated team member triggers Create from S-02 |
| **Type** | Modal / overlay |
| **Exit** | Successful Create → S-03; invalid submission remains M-01; close → previous S-02 state |

**Source:** US-01 / AC-01.1–AC-01.5; BR-02–BR-04; **D-01, D-04, D-07, D-09**.

## 5.2 User-entered fields vs automatic values

### User-entered fields

| Field | Required | Allowed content | Missing value appearance |
|---|---|---|---|
| **Title** | Yes | Text | Empty input before valid submission |
| **Description** | Yes | Any text or link | Empty input before valid submission |

**Description format:** no separate UI format validation; text or link accepted. **D-09.**

### Automatically assigned after Create

| Field | Automatically set | Value |
|---|---|---|
| **ID** | Yes | Unique ID |
| **Status** | Yes | `Open` |
| **Created at** | Yes | Current creation timestamp |
| **Author** | Yes | Current authenticated user |
| **Priority** | Yes | `Not Set` |
| **Assignee** | Yes | Absent after creation |

**Source:** AC-01.4–AC-01.5, BR-03–BR-04, **D-03, D-04**.

## 5.3 Controls

| C-ID | Label | Type | Visible conditions | Enabled conditions | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-06 | Title | Text input | Always in M-01 | User can enter text | Enter | Value retained | AC-01.1–01.2 |
| C-07 | Description | Multiline text input | Always in M-01 | User can enter text/link | Enter | Value retained | AC-01.1, AC-01.3; **D-09** |
| C-08 | Create | Button | Always | Enabled for submission attempt | Click | Valid → S-03; invalid → M-01 | US-01 scenarios |
| C-09 | Close | Button/control | Modal open | Enabled | Close | Previous S-02 state | **D-07** |

`C-06`, `C-07`, `C-08`, `C-09` — widget types are UX details not explicitly named by the source.

**PROPOSED D-15:** use text input for Title, multiline text input for Description, button for Create, and a close control for modal dismissal. This does not change functionality.

## 5.4 Default state

The source does **not** define exact initial field values for Title/Description.

**Contract:** fields exist and are required; exact initial rendering is **PROPOSED D-15**, not SOURCE.

## 5.5 Validation state

### Title empty

**Expected:**
- issue is not created;
- error text **`Title is required`** is displayed;
- remain on M-01.

**Source:** US-01 / scenario for АС-02.

### Description empty

AC-01.3 requires Description, but no exact source error copy is specified.

**PROPOSED D-17:** define exact Description-required error copy.  
**Status:** not an approved requirement.

### Description as link

No separate format validation is required.

**Source/D-ID:** D-09.

## 5.6 Error states

The source specifies no generic create-save failure behavior beyond the Title-required scenario.

Do not invent a generic “Save failed” requirement.

---

# 6. S-03 — Issue Details

## 6.1 Contract

| Field | Contract |
|---|---|
| **ID** | S-03 |
| **Purpose** | Показ повних issue details і виконання дозволених assignment, priority, status та comment interactions |
| **Actors** | Team Member, Team Lead |
| **Entry conditions** | Issue selected from S-02 or issue successfully created from M-01 |
| **Visible data** | ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at, comments when present |
| **Exit** | Back → exact previous S-02 list state |

**Source:** US-02 / AC-02.4; US-03; US-04; US-05; US-06; US-07; **D-03–D-07, D-14**.

## 6.2 Visible data

| Field / data | Requiredness | Missing-value appearance |
|---|---|---|
| **ID** | Required | Must be present for a created issue |
| **Title** | Required at creation | No valid created issue should lack it |
| **Description** | Required at creation | No valid created issue should lack it |
| **Status** | Required | Must always show current status |
| **Priority** | Optional until Manager sets it | `Not Set` |
| **Assignee** | Conditional | Absent until assigned |
| **Author** | Required after creation | Must be present |
| **Created at** | Required after creation | Must be present |
| **Comment text** | Required only for a created comment | No comment item if none exists |
| **Comment author** | Required for stored comment | Must be displayed on stored comment |
| **Comment creation date/time** | Required for stored comment | Must be displayed on stored comment |

**Date/time display:** standard format, e.g. `DD.MM.YYYY HH:MM`, no timezone controls in mockup. **D-14.**

## 6.3 Controls

### Assignment

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-10 | Assignee | Selector | Always on S-03 | Team Lead: enabled; other users: disabled | Select team member | Selection held | US-03; BR-06 not applicable |
| C-11 | Apply | Button | With assignee control | Team Lead only | Click | Assignee applied; remain S-03 | **D-06** |

**Source constraints:** one assignee; assignee must be team member; only Manager can assign; current assignee shown. Source has no numeric AC IDs for these four bullets.

### Priority

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-12 | Priority | Selector | Always | Manager enabled; others disabled | Select Low/Normal/High/Critical | Selection held | AC-04.1–04.2; **D-03, D-05** |
| C-13 | Apply | Button | With priority control | Manager only | Click | Priority applied; remain S-03 | **D-06** |

**Important distinction:**
- absent current value = **Not Set**;
- allowed values that may be set = `Low / Normal / High / Critical`;
- reset action is not provided.

**Source:** BR-04, BR-05; US-04; **D-03**.

### Status

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-14 | Status | Selector | Always | Current Assignee enabled; all non-assignees disabled | Select permitted next status | Apply candidate transition | BR-07, BR-08.1; **D-05, D-06** |
| C-15 | Apply | Button | With status control | Current Assignee only | Click | If allowed → state changes; else no transition | BR-08.1; **D-06** |

**Team Lead does not receive status rights automatically.** Status permission depends on assignee relationship, not global role.

### Comments

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-16 | Comment text | Multiline text input | Always | Team member enabled | Enter | Text retained | US-06 / AC-06.1 |
| C-17 | Add Comment | Button | With comment input | Team member | Click | Success → stored/displayed comment; failure → error, no new comment | US-06 / AC-06.2–06.5 |

**PROPOSED D-18:** use a multiline input for comment text. No new functionality is introduced.

### Back

| C-ID | Label | Type | Visible | Enabled | Trigger | Result | Source/D-ID |
|---|---|---|---|---|---|---|---|
| C-18 | Back | Button/control | Always | Enabled | Click | Return to previous S-02 state | **D-07** |

Exact visual label is not in the source.

**PROPOSED D-19:** label the return control `Back`.

## 6.4 Role/state conditions

### Assignment
- Team Lead: enabled.
- Non-Team-Lead: disabled.
- Current assignee relation does not grant assignment permission.

### Priority
- Team Lead: enabled.
- Translator: disabled.
- Assignee relation does not grant priority permission.

### Status
- Current Assignee: enabled, regardless of whether user is Team Member or Team Lead.
- Non-Assignee: disabled, regardless of role.
- Team Lead does **not** automatically gain status rights.

### Comment
- Any authenticated team member can add comments; no source rule restricts comments by assignee or role.

**Source:** BR-02, BR-06, BR-08; US-03, US-04, US-06, US-07.

## 6.5 Disabled state

**Approved behavior:** role-dependent forbidden controls remain **visible and disabled**, not hidden.

**Source/D-ID:** **D-05**.

## 6.6 Comment states

### Empty comment
`Comment text` is empty → comment must not be created.

**Source:** AC-06.1.

### Successful save
Comment is stored and shown, with author and creation date/time.

**Source:** AC-06.2–AC-06.4.

### Failed save
Comment is **not created** and an error is produced.

**Source:** AC-06.5.

**PROPOSED D-20:** exact text of empty-comment/error messages requires a decision. No product behavior is added by this proposal.

---

# 7. Permission matrix

**Assumption for this matrix:** “assignee” is a relationship to the current issue, not a separate global role.

| Action | Team Member — non-assignee | Team Member — assignee | Team Lead — non-assignee | Team Lead — assignee |
|---|---:|---:|---:|---:|
| **view** | Allow | Allow | Allow | Allow |
| **create** | Allow | Allow | Allow | Allow |
| **comment** | Allow | Allow | Allow | Allow |
| **assign** | Deny / disabled | Deny / disabled | **Allow** | **Allow** |
| **change priority** | Deny / disabled | Deny / disabled | **Allow** | **Allow** |
| **change status** | Deny / disabled | **Allow** | Deny / disabled | **Allow** |

### Matrix basis

- View: authorized team member may view issues — US-02.
- Create: any authenticated team member — BR-02.
- Assign: only Manager / Team Lead — US-03.
- Priority: only Manager / Team Lead — BR-06, AC-04.2.
- Status: only responsible/assigned member — BR-08, AC-07.4.
- Comment: team member may add comments — US-06.

**Important:** `Team Lead = assignee` is not equivalent to “Team Lead automatically has status rights”; status right comes from **assignee relation**.

---

# 8. Full status transition matrix

Allowed source transitions are exactly those defined in **BR-08.1**.

| From \ To | Open | In Progress | Resolved | Closed |
|---|---|---|---|---|
| **Open** | — current status, not a transition | **ALLOW** | **FORBIDDEN** | **ALLOW** only for cancellation |
| **In Progress** | **FORBIDDEN** | — current status, not a transition | **ALLOW** | **FORBIDDEN** |
| **Resolved** | **FORBIDDEN** | **ALLOW** reopen work | — current status, not a transition | **ALLOW** |
| **Closed** | **FORBIDDEN** | **FORBIDDEN** | **FORBIDDEN** | — current status; no outgoing transitions |

### Exact allowed transitions

1. `Open → In Progress`
2. `In Progress → Resolved`
3. `Resolved → Closed`
4. `Resolved → In Progress`
5. `Open → Closed` for client cancellation before processing

**Closed has no outgoing transitions.**

**Current status is displayed state, not a transition.** Selecting/seeing the current status does not create a new transition.

**All other pairs are forbidden.**

### Actor condition

For all allowed transitions, the executor must satisfy status permission: **Current Assignee**.

For `Open → Closed` cancellation, D-12 explicitly assigns this action to the **Current Assignee**.

---

# 9. Important predicate

## Exact rule

```text
Important(issue) =
    priority ∈ {High, Critical}
    AND
    status != Closed
```

**Source:** AC-08.1–AC-08.4; BR-09/BR-09.1; **D-10**.

### Examples

| Priority | Status | Result |
|---|---|---|
| High | Resolved | **Important** |
| Critical | Closed | **Not Important** |
| Not Set | Open | **Not Important** |
| Not Set | Resolved | **Not Important** |
| Low | Open | **Not Important** |
| Normal | In Progress | **Not Important** |
| High | Closed | **Not Important** |
| Critical | In Progress | **Important** |

No UI rule may replace `status != Closed` with `status = Open`.

---

# 10. Incoming / outgoing transition map

| Screen/State | Incoming T-ID | Outgoing T-ID |
|---|---|---|
| **S-01** | — | T-01 |
| **S-02-A** | T-01, T-18 from All | T-02, T-04, T-08, T-19 |
| **S-02-B** | T-02, T-18 from Important | T-03, T-08, T-19 |
| **S-02-C** | T-19 | T-02/T-03 via tab, T-04 |
| **M-01** | T-04 | T-05 success; T-06/T-07 remain same modal state; close → previous S-02 state |
| **S-03** | T-05, T-08 | T-09/T-10/T-11/T-12/T-13/T-14/T-15/T-16/T-17; T-20 |
| **No transition** | — | Forbidden actions T-21/T-22/T-23 |

---

# 11. Developer verification by surface

## S-01

**Mockup verifies:** boundary placeholder exists; no fabricated login screen.  
**Prototype verifies:** demo can start after authentication boundary.  
**Implementation verifies:** actual authentication enforcement.

**Cannot be proved by mockup:** authentication itself.

---

## S-02-A / S-02-B / S-02-C

**Mockup verifies:**
- required columns/data;
- All/Important tabs;
- Not Set representation;
- Important state composition;
- empty state message presence;
- Create control;
- no separate Assignment/Priority/Status/Comment screen.

**Prototype verifies:**
- All ↔ Important;
- row → S-03;
- Create → M-01;
- empty-state navigation;
- preserved list state after Back.

**Implementation verifies:**
- actual issue retrieval;
- actual Important predicate;
- actual persistence;
- actual access authorization;
- actual response time.

---

## M-01

**Mockup verifies:**
- Title and Description fields;
- required markers/validation state;
- Create control;
- absence of assignee/priority/status as user inputs.

**Prototype verifies:**
- Create interaction;
- Title empty error;
- Description required behavior;
- successful result → S-03;
- modal dismissal.

**Implementation verifies:**
- unique ID;
- persisted issue;
- current user as author;
- `Open`;
- `Not Set`;
- timestamp persistence.

---

## S-03

**Mockup verifies:**
- all required detail data;
- Not Set;
- absent assignee;
- role-disabled controls;
- Apply controls;
- comment success/error states;
- status control states;
- no delete/history UI.

**Prototype verifies:**
- assignment → Apply → remain S-03;
- priority → Apply → remain S-03;
- allowed status transitions;
- reopen Resolved;
- cancellation Open → Closed;
- comment success/error;
- disabled unauthorized actions produce no transition;
- Back preserves previous list state.

**Implementation verifies:**
- role enforcement;
- assignee-based status authorization;
- actual persistence;
- actual comment save failure semantics;
- actual transition enforcement;
- actual stored author/time;
- actual data consistency.

---

# 12. Test catalogue

## Test definitions

| Test ID | Actor / context | Precondition | Action | Expected visible result | Cannot be proved by this test | Level |
|---|---|---|---|---|---|---|
| **TEST-01** | Authenticated Translator | Auth boundary satisfied | Enter MVP | S-02-A available | Real authentication | M/P |
| **TEST-02** | Translator | S-02-A/B/C open | Create with valid Title + Description | M-01 accepts values; successful predetermined result can lead to S-03 | Unique ID/persistence/current backend user | P/I |
| **TEST-03** | Translator | M-01 open | Leave Title empty → Create | `Title is required`; issue not created; M-01 remains | Backend rejection independently | M/P |
| **TEST-04** | Translator | M-01 open | Leave Description empty → Create | Creation blocked; exact error copy not yet fixed | Exact implementation error wording | M/P |
| **TEST-05** | Translator | M-01 open | Enter plain text, then link in Description | No separate format-validation state | Actual backend acceptance | P/I |
| **TEST-06** | Team Member | Issues exist | Open S-02-A | ID/Title/Status/Priority/Assignee-if-set visible | Real data retrieval | M/P/I |
| **TEST-07** | Team Member | Issue selected | Open issue | S-03 shows ID/Title/Description/Status/Priority/Assignee/Author/Created at | Actual persistence | M/P |
| **TEST-08** | Team Member | No applicable issues | Open All or Important | S-02-C shows no-issues message, not empty table | Backend query correctness | M/P |
| **TEST-09** | Team Lead | S-03, no assignee | Select team member → Apply | Current assignee displayed; remain S-03 | Actual persistence | P/I |
| **TEST-10** | Team Lead | S-03 | Select priority Low/Normal/High/Critical → Apply | Selected priority displayed; remain S-03 | Backend authorization/persistence | M/P/I |
| **TEST-11** | Team Member | S-03 | Inspect status in list/details | Current status visible | Actual synchronization from backend | M |
| **TEST-12** | Current Assignee | Status = Open | Open → In Progress → Apply | Status becomes In Progress | Backend enforcement | P/I |
| **TEST-13** | Current Assignee | Status = In Progress | In Progress → Resolved → Apply | Status becomes Resolved | Persistence unless implementation test | P/I |
| **TEST-14** | Current Assignee | Status = Resolved | Resolved → In Progress → Apply | Status returns to In Progress | Persistence | P/I |
| **TEST-15** | Current Assignee | Status = Resolved | Resolved → Closed → Apply | Status becomes Closed; issue no longer Important | Actual data update | P/I |
| **TEST-16** | Current Assignee | Status = Open; cancellation condition | Open → Closed → Apply | Status becomes Closed | Whether cancellation event is valid externally | P/I |
| **TEST-17** | Non-Assignee | Any editable status | Attempt status change | Control is visible disabled; no transition | Backend authorization | M/P |
| **TEST-18** | Any actor | Current status known | Attempt forbidden status transition | No transition; status remains unchanged | Exact UI handling of forbidden option | P/I |
| **TEST-19** | Team Member | S-03 | Add non-empty comment; save succeeds | Comment displayed with author/date-time | Actual storage unless I | P/I |
| **TEST-20** | Team Member | S-03 | Try empty comment | Comment not created | Backend enforcement unless I | M/P |
| **TEST-21** | Team Member | S-03 | Comment save fails | Error; no comment created | Real failure injection/backend behavior | P/I |
| **TEST-22** | Team Member | Important candidates exist | Open Important | High/Critical + non-Closed shown; Low/Normal and Closed excluded | Actual query correctness | M/P/I |
| **TEST-23** | Team Member | High + Resolved issue exists | Open Important | Issue is visible | Backend predicate execution | M/P |
| **TEST-24** | Team Member | Critical + Closed issue exists | Open Important | Issue is absent | Backend predicate execution | M/P |
| **TEST-25** | Team Member | Priority = Not Set | Open Important | Issue is absent | Backend query | M/P |
| **TEST-26** | Team Member | Previous tab = Important | Open issue → Back | Return to S-02-B | Implementation of navigation state beyond prototype | P/I |
| **TEST-27** | Team Member non-assignee | S-03 | Try Assign/Priority/Status | Controls visible disabled; no state change | Backend permission enforcement | M/P/I |
| **TEST-28** | Team Lead non-assignee | S-03 | Try Status | Status control visible disabled | Backend permission enforcement | M/P/I |
| **TEST-29** | Team Lead assignee | S-03 | Change Status | Status control enabled | Actual role/relationship calculation | M/P/I |
| **TEST-30** | Team Lead non-assignee | S-03 | Assign / Priority | Controls enabled | Backend authorization | M/P/I |
| **TEST-31** | Any user | S-03 with dates | Inspect Created at/comment time | Standard format such as DD.MM.YYYY HH:MM | Backend timezone semantics | M/P |
| **TEST-32** | Demo user | Prototype data | Run complete demo | Predetermined results; no real login/DB interaction | Production infrastructure | P |
| **TEST-33** | Any state | Status = Closed | Attempt any outgoing status transition | No transition | Backend enforcement | P/I |

---

# 13. Traceability

| Source ID / point | UIR-ID | Screen/State | C-ID / annotation | Test ID | Verification level |
|---|---|---|---|---|---|
| BR-01 | UIR-001 | S-01 | A-01 Authentication boundary | TEST-01 | M/P/I |
| NFR-02 | UIR-002 | S-01 | A-01 | TEST-01 | I |
| US-01 / AC-01.1–AC-01.5 | UIR-003 | M-01 | C-06/C-07/C-08 + A-02 | TEST-02 | P/I |
| US-01 / Title-empty scenario | UIR-004 | M-01 | C-06/C-08 | TEST-03 | M/P |
| US-01 / AC-01.3 | UIR-005 | M-01 | C-07/C-08 | TEST-04 | M/P |
| D-09 | UIR-006 | M-01 | C-07 | TEST-05 | M/P |
| US-02 / AC-02.1–AC-02.2 | UIR-007 | S-02-A/B | C-02/C-03/C-04 | TEST-06 | M/P |
| US-02 / AC-02.3–AC-02.4 | UIR-008 | S-03 | C-04 | TEST-07 | M/P |
| US-02 / AC-02.5 | UIR-009 | S-02-C | A-03 Empty state | TEST-08 | M/P |
| US-03 / first bullet: “можна вибрати одного assignee” | UIR-010 | S-03 | C-10/C-11 | TEST-09 | P/I |
| US-03 / second bullet: “assignee повинен бути членом команди” | UIR-011 | S-03 | C-10 | TEST-09 | P/I |
| US-03 / third bullet: “лише Manager” | UIR-012 | S-03 | C-10/C-11 | TEST-27/T30 | M/P/I |
| US-03 / fourth bullet: current assignee displayed | UIR-013 | S-03 | C-10 | TEST-09 | M/P |
| US-04 / АС-04.1–04.2 | UIR-014 | S-03 | C-12/C-13 | TEST-10 | M/P/I |
| US-05 / АС-05.1–AC-05.3 | UIR-015 | S-02-A/B, S-03 | C-04/A-04 | TEST-11 | M/P |
| US-06 / AC-06.1 | UIR-016 | S-03 | C-16 | TEST-20 | M/P |
| US-06 / AC-06.2–AC-06.4 | UIR-017 | S-03 | C-17 | TEST-19 | P/I |
| US-06 / AC-06.5 | UIR-018 | S-03 | C-17 | TEST-21 | P/I |
| US-07 / АС-07.1–07.2 | UIR-019 | S-03 | C-14/C-15 | TEST-12/T13 | M/P |
| BR-08.1 / Open→In Progress | UIR-020 | S-03 | C-14/C-15 | TEST-12 | P/I |
| BR-08.1 / In Progress→Resolved | UIR-021 | S-03 | C-14/C-15 | TEST-13 | P/I |
| BR-08.1 / Resolved→In Progress | UIR-022 | S-03 | C-14/C-15 | TEST-14 | P/I |
| BR-08.1 / Resolved→Closed | UIR-023 | S-03 | C-14/C-15 | TEST-15 | P/I |
| BR-08.1 / Open→Closed cancellation | UIR-024 | S-03 | C-14/C-15 | TEST-16 | P/I |
| US-07 / AC-07.4 | UIR-025 | S-03 | C-14 | TEST-17/T28 | M/P/I |
| US-07 / AC-07.5 | UIR-026 | S-03 | C-14/C-15 | TEST-18 | P/I |
| US-08 / AC-08.1–AC-08.4 | UIR-027 | S-02-B | C-03/C-04/A-05 | TEST-22 | M/P/I |
| BR-09.1 | UIR-028 | S-02-B | A-05 Important predicate | TEST-23 | M/P |
| D-03 | UIR-029 | S-02/S-03/M-01 | C-12/A-06 | TEST-10/T25 | M/P |
| D-04 | UIR-030 | M-01/S-03 | A-07 Initial assignee absent | TEST-02/T09 | M/P |
| D-05 | UIR-031 | S-03 | A-08 Disabled role control | TEST-27/T28/T30 | M/P/I |
| D-06 | UIR-032 | S-03 | C-11/C-13/C-15 | TEST-09/T10/T12 | P/I |
| D-07 | UIR-033 | S-03 → S-02 | C-18/A-09 Previous List State | TEST-26 | P/I |
| D-08 | UIR-034 | All demo states | A-10 Demo data | TEST-32 | P |
| D-14 | UIR-035 | S-03 | A-11 Date-time format | TEST-31 | M/P |

---

# 14. Acceptance Criteria coverage check

## US-01
- **AC-01.1** → TEST-02
- **AC-01.2** → TEST-02, TEST-03
- **AC-01.3** → TEST-02, TEST-04
- **AC-01.4** → TEST-02
- **AC-01.5** → TEST-02, TEST-10/T29

Сценарій Title empty → **TEST-03**.

## US-02
- **AC-02.1** → TEST-06
- **AC-02.2** → TEST-06
- **AC-02.3** → TEST-07
- **AC-02.4** → TEST-07
- **AC-02.5** → TEST-08

## US-03
- ненумерований пункт 1: one assignee → **TEST-09**
- ненумерований пункт 2: assignee is team member → **TEST-09**
- ненумерований пункт 3: only Manager → **TEST-27, TEST-30**
- ненумерований пункт 4: current assignee displayed → **TEST-09**

## US-04
- **АС-04.1** → TEST-10
- **АС-04.2** → TEST-10, TEST-27, TEST-30

## US-05
- **АС-05.1** → TEST-11
- **AC-05.2** → TEST-11
- **AC-05.3** → TEST-06, TEST-11

## US-06
- **AC-06.1** → TEST-19, TEST-20
- **AC-06.2** → TEST-19
- **AC-06.3** → TEST-19
- **AC-06.4** → TEST-19
- **AC-06.5** → TEST-21

## US-07
- **АС-07.1** → TEST-02, TEST-12
- **АС-07.2** → TEST-12/T13
- **АС-07.3** → TEST-12/T13/T14/T15/T16 + implementation verification
- **АС-07.4** → TEST-17, TEST-28
- **АС-07.5** → TEST-18

## US-08
- **AC-08.1** → TEST-22, TEST-23
- **AC-08.2** → TEST-22
- **AC-08.3** → TEST-22
- **AC-08.4** → TEST-22, TEST-24

---

# 15. What is NOT a product screen

The following remain inside S-03 and must not become independent screens:

- Assignment;
- Priority;
- Status;
- Comments.

Their implementation is represented by C-10…C-17 and component states.

Also not separate screens:
- disabled role-dependent controls;
- comment save error;
- Not Set;
- status transition states;
- previous-list return behavior;
- Important predicate examples;
- authentication placeholder.

---

# 16. Required business / UX decisions still not fixed

Ці пункти не змінюють screen inventory, але для повністю детермінованого developer-level UI contract потрібне окреме рішення.

### PROPOSED D-15 — Control widget types
Визначає конкретний widget type для полів і selectors:
- Title → text input;
- Description/Comment → multiline input;
- Assignee/Priority/Status → selector;
- Create/Apply/Back → buttons/controls.

**Status:** PROPOSED, не SOURCE.

### PROPOSED D-16 — Exact empty-state copy
Потрібно затвердити конкретний текст для S-02-C.

**Status:** UNRESOLVED.

### PROPOSED D-17 — Description-required error copy
Потрібно затвердити конкретний error message для порожнього Description.

**Status:** UNRESOLVED.

### PROPOSED D-18 — Comment widget type
Потрібно остаточно визначити presentation comment input, якщо це має бути формалізовано на рівні component contract.

**Status:** PROPOSED.

### PROPOSED D-19 — Back control label
Поведінка Back погоджена D-07, але конкретний visual label не заданий.

**Status:** PROPOSED.

### PROPOSED D-20 — Comment empty/save-error copy
Потрібні конкретні тексти для:
- порожнього comment;
- failed comment save.

**Status:** UNRESOLVED.

### Interaction detail still not specified
BR-08.1 забороняє інші status transitions, але не визначає, чи недозволений destination:
- відсутній із selector;
- показується disabled;
- або допускається вибір із подальшим блокуванням Apply.

Це **не слід вигадувати як SOURCE**.

**Потрібне окреме рішення людини**, наприклад **PROPOSED D-21 — presentation of forbidden status destinations**.

---

# 17. Verification status

Жоден test у цьому документі не має статусу **PASS**.

Поточний документ визначає:
- expected contract;
- source trace;
- approved design basis;
- proposed UX details;
- verification level.

Фактичний результат **PASS / FAIL** можливий лише після відповідного mockup, prototype або implementation test.

## Final boundary

Цей контракт **не додає нових продуктових функцій, ролей, полів, способів authentication, screens або persistence mechanisms**.

Release MVP залишається **US-01–US-08**; US-09 — Deferred; SR-10/SR-11 — Future.

Поточна screen architecture залишається:

**S-01 → S-02-A/B/C ↔ M-01 → S-03 ↔ S-02(previous state)**

Status, Assignment, Priority та Comments не виділяються в самостійні продуктові екрани.