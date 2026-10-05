# VerbaDesk — Critical Review of Screen Plan and Contracts

## 0. Review basis

Перевірено:

1. `Translation_Spec.docx`, зокрема §3 Business Rules, §5 User Stories + AC, §6 NFR, §8 final Release Scope.
2. APPROVED D-01…D-14.
3. попередній Input Pack v1.0;
4. Screen Contracts v1.0;
5. Transition Matrix;
6. Permission Matrix;
7. Traceability.

Фінальний Release Scope джерела: **US-01…US-08 — Release MVP; US-09 — Deferred; SR-10/SR-11 — Future.**

---

# 1. Coverage matrix — every AC

**Статуси:**

- **COVERED** — вимога присутня в Screen Contracts/flow/test plan без зміни змісту.
- **AMBIGUOUS** — вимога присутня, але спосіб її developer verification або межа між SOURCE і UX не достатньо чіткі.
- **MISSING** — вимога не має достатнього покриття в плані.

## US-01 — Create Issue

Джерело: §5 / US-01 / AC-01.1–AC-01.5 та два наведені сценарії.

| AC | Status | Evidence in plan |
|---|---|---|
| **AC-01.1** User can enter Title, Description | **COVERED** | M-01 C-06/C-07; §5.2 |
| **AC-01.2** Title required | **COVERED** | M-01 C-06; validation state; TEST-03 |
| **AC-01.3** Description required | **COVERED** | M-01 C-07; TEST-04 |
| **AC-01.4** After creation: ID, Open, created_at, author | **COVERED** | §5.2 automatic values + S-03 visible data + TEST-02 |
| **AC-01.5** Priority initially not set | **COVERED** | M-01 §5.2 + C-01/Not Set + D-03 |

**Review note:** AC-01.4 фактично заданий правильно. Однак TEST-02 сформульований занадто загально (`predetermined result can lead to S-03`) і не перераховує всі чотири автоматичні результати. Це недолік тестового запису, а не пропуск вимоги.

---

## US-02 — View Issues

Джерело: §5 / US-02 / AC-02.1–AC-02.5.

| AC | Status | Evidence in plan |
|---|---|---|
| **AC-02.1** Authenticated team member can view list | **COVERED** | S-02-A/B entry conditions; TEST-06 |
| **AC-02.2** List shows ID, Title, Status, Priority, Assignee if assigned | **COVERED** | S-02-A/B visible data |
| **AC-02.3** Select issue and open details | **COVERED** | C-04; T-08; TEST-07 |
| **AC-02.4** Details show ID, Title, Description, Status, Priority, Assignee, Author, Created at | **COVERED** | S-03 visible data; TEST-07 |
| **AC-02.5** Empty list → empty list or message | **COVERED** | S-02-C + D-11; TEST-08 |

---

## US-03 — Assign Issue

Джерело містить **чотири ненумеровані** Acceptance Criteria; окремі AC-ID не створювалися.

| Source point | Status | Evidence in plan |
|---|---|---|
| one assignee can be selected | **COVERED** | S-03 C-10/C-11; TEST-09 |
| assignee must be team member | **COVERED** | C-10 condition; TEST-09 |
| only Manager can assign | **COVERED** | Permission Matrix + D-05; TEST-27/T30 |
| current assignee displayed | **COVERED** | S-03 visible data; TEST-09 |

**Traceability:** оригінальні ненумеровані source points збережені; вигаданих `AC-03.x` не створено.

---

## US-04 — Set Issue Priority

Джерело: §5 / US-04 / АС-04.1–АС-04.2.

| AC | Status | Evidence in plan |
|---|---|---|
| **АС-04.1** Low / Normal / High / Critical | **COVERED** | C-12; D-03; TEST-10 |
| **АС-04.2** only Manager can set/change | **COVERED** | Permission Matrix + C-12/C-13 disabled rule; TEST-27/T30 |

---

## US-05 — Track Issue Progress

Джерело: §5 / US-05 / АС-05.1, AC-05.2, AC-05.3.

| AC | Status | Evidence in plan |
|---|---|---|
| **АС-05.1** current status displayed for each issue | **COVERED** | S-02-A/B visible data; TEST-11 |
| **AC-05.2** status displayed on issue page | **COVERED** | S-03 visible data; TEST-11 |
| **AC-05.3** status displayed in list | **COVERED** | S-02-A/B visible data; TEST-06/11 |

---

## US-06 — Add Comment

Джерело: §5 / US-06 / AC-06.1–AC-06.5.

| AC | Status | Evidence in plan |
|---|---|---|
| **AC-06.1** Comment text cannot be empty | **COVERED** | C-16 + TEST-20 |
| **AC-06.2** Stored and shown | **COVERED** | C-17 + S-03 + TEST-19 |
| **AC-06.3** Author stored/displayed | **COVERED** | S-03 comment data + TEST-19 |
| **AC-06.4** Creation date/time stored/displayed | **COVERED** | S-03 + D-14 + TEST-19 |
| **AC-06.5** Failed save → no comment + error | **COVERED** | C-17 error state + TEST-21 |

Особливо важлива негативна умова AC-06.5 не втрачена.

---

## US-07 — Update Issue Status

Джерело: §5 / US-07 / АС-07.1–АС-07.5.

| AC | Status | Evidence in plan |
|---|---|---|
| **АС-07.1** New issue starts Open | **COVERED** | M-01 automatic values; S-03; TEST-02 |
| **АС-07.2** Allowed statuses only Open/In Progress/Resolved/Closed | **COVERED** | C-14 + full status matrix |
| **АС-07.3** Changed value is stored and shown on next view | **AMBIGUOUS** | T-11…T-16 + TEST-12…16 show change, але окремого test sequence “change → leave/reopen → verify persisted value” немає |
| **АС-07.4** Non-assignee cannot change status | **COVERED** | Permission Matrix; C-14; TEST-17/T28 |
| **АС-07.5** System does not allow status value outside defined list | **AMBIGUOUS** | C-14 обмежує selector дозволеним переліком, але немає окремого implementation test на invalid/out-of-enum value |

**Ключовий дефект:** AC-07.5 не тотожний забороні неправильного transition. `Resolved → Open` і “status = UnknownValue” — різні перевірки.

---

## US-08 — View Important Open Issues

Джерело: §5 / US-08 / AC-08.1–AC-08.4.

| AC | Status | Evidence in plan |
|---|---|---|
| **AC-08.1** High + non-Closed appears | **COVERED** | exact predicate + TEST-22/23 |
| **AC-08.2** Critical + non-Closed appears | **COVERED** | exact predicate + TEST-22 |
| **AC-08.3** Low/Normal excluded | **COVERED** | exact predicate + TEST-22 |
| **AC-08.4** Closed excluded regardless of priority | **COVERED** | exact predicate + TEST-22/24 |

`High + Resolved` correctly included.  
`Critical + Closed` correctly excluded.  
`Not Set` correctly excluded because it is neither High nor Critical.

---

# 2. DEF-ID — defects found

## DEF-01 — HIGH — unapproved navigation represented as SOURCE

**Source:** US-01 / AC-01.4 establishes that an issue is created with required data; it does **not** state that successful Create navigates to S-03.

**Plan location:** M-01 exit; T-05; TEST-02.

**Problem:**  
`M-01 → S-03` after successful creation was recorded as if it followed directly from US-01 / AC. This is an UX/navigation decision, not a source requirement and not one of D-01…D-14.

**Minimum fix:**  
Mark successful Create → S-03 as **PROPOSED D-ID** or leave its destination unresolved. It must not be cited as SOURCE.

**Type:** requirement/traceability defect, not merely cosmetic UX.

---

## DEF-02 — MEDIUM — modal close route is not approved

**Source:** D-01 establishes a Create modal; D-07 establishes previous-list preservation when returning from details. Neither explicitly defines M-01 dismissal behavior.

**Plan location:** M-01 `C-09 Close`; M-01 Exit; T-04/T-07.

**Problem:**  
`M-01 → previous S-02 state` was treated as established behavior.

**Minimum fix:**  
Mark modal dismissal as **PROPOSED D-ID**, not SOURCE/D-07.

**Type:** UX proposal improperly presented as a contract fact.

---

## DEF-03 — HIGH — AC-07.3 lacks a real “next view” verification

**Source:** AC-07.3 explicitly requires that after status change the new value is **stored** and displayed on the **next viewing**.

**Plan location:** TEST-12…TEST-16.

**Problem:**  
Those tests verify immediate visible change while staying on S-03. That does not by itself verify “next view” persistence.

**Minimum fix:** add an implementation test:

`Current Assignee → change status → leave S-03 → reopen same issue → expected new status is still displayed`.

**Type:** genuine requirement coverage defect.

---

## DEF-04 — HIGH — AC-07.5 is not independently tested

**Source:** AC-07.5 says the system must not allow a status value outside the defined list.

**Plan location:** C-14; TEST-18.

**Problem:**  
TEST-18 checks a **forbidden transition**, not an **invalid status value**. Those are separate conditions.

**Minimum fix:** add an implementation-level test attempting an out-of-enum status value and verify rejection/no invalid persisted state.

No new UI control is required.

**Type:** genuine requirement coverage defect.

---

## DEF-05 — MEDIUM — Description-empty behavior mixes source and UX interpretation

**Source:** AC-01.3 only says Description is required.

**Plan location:** M-01 validation; T-07; TEST-04.

**Problem:**  
The plan correctly retains the requirement, but the statement “remain on M-01” is a UX decision, not explicitly supplied by the source. Exact error text is also not source-defined.

**Minimum fix:**  
Keep **AC-01.3 = SOURCE**. Mark “remain M-01” and any exact error copy as **PROPOSED D-ID**. Do not invent source wording.

**Type:** requirement-vs-UX classification defect.

---

## DEF-06 — MEDIUM — forbidden transition presentation is unresolved

**Source:** BR-08.1 says all transitions other than the listed ones are not allowed. It does not specify how an unavailable destination must visually appear.

**Plan location:** C-14; T-23; §16 / proposed D-21.

**Problem:**  
The plan correctly specifies **no transition**, but does not have an approved presentation rule for the invalid destination.

**Minimum fix:** keep the business rule as SOURCE; keep visual presentation as **UNRESOLVED / PROPOSED D-ID**.

This does **not** justify adding new product behavior.

---

## DEF-07 — MEDIUM — Closed status and enabled C-14 are not fully reconciled

**Source:** BR-08.1 permits no outgoing transitions from Closed. The plan also states C-14 is enabled for Current Assignee.

**Plan location:** S-03 C-14 + full status matrix.

**Problem:**  
An enabled status control on Closed has no permitted outgoing status transition. The contract does not say what the control does in that state.

The business rule itself is correct; the ambiguity is purely in the UI interaction contract.

**Minimum fix:**  
Do not change BR-08.1. Mark the Closed-state presentation of C-14 as **UNRESOLVED / PROPOSED D-ID**.

---

# 3. What was checked and found correct

## Permissions

The matrix correctly distinguishes relationship-to-issue from global role:

| Actor/context | View | Create | Comment | Assign | Priority | Status |
|---|---:|---:|---:|---:|---:|---:|
| Team Member — non-assignee | Allow | Allow | Allow | Deny | Deny | Deny |
| Team Member — assignee | Allow | Allow | Allow | Deny | Deny | **Allow** |
| Team Lead — non-assignee | Allow | Allow | Allow | **Allow** | **Allow** | **Deny** |
| Team Lead — assignee | Allow | Allow | Allow | **Allow** | **Allow** | **Allow** |

Це відповідає BR-02, BR-06, BR-08 та US-03/04/06/07.

**Особливо:** Team Lead non-assignee не отримує status rights автоматично. Це правильно.

---

## Status workflow

Повна матриця не містить незаконних дозволених переходів.

**Allowed only:**

- Open → In Progress
- In Progress → Resolved
- Resolved → Closed
- Resolved → In Progress
- Open → Closed при cancellation before processing

`Closed` не має outgoing transitions.

**Source:** BR-08.1.

`Current status` правильно трактується як стан, а не transition.

---

## Important predicate

План використовує правильне точне правило:

`priority ∈ {High, Critical} AND status != Closed`

Тому:

- High + Resolved → **включити**;
- Critical + Closed → **виключити**;
- Not Set + Open → **виключити**.

Це відповідає AC-08.1–AC-08.4 та BR-09.1.

---

## Not Set

План правильно розділяє:

**відсутнє значення:** `Not Set`  
**values allowed to be set:** `Low / Normal / High / Critical`

Reset не додається.

**Source:** BR-04/BR-05; **D-03**.

---

## Create

План правильно розділяє:

**User-entered:**  
- Title
- Description

**Automatically assigned:**  
- ID
- Open
- Created at
- Author
- Not Set
- no assignee

Тому Create не перетворений на форму для priority/status/assignee.

---

## Comments

План не втрачає жодної негативної умови:

- empty comment → no creation;
- success → stored + shown;
- author shown;
- creation time shown;
- failed save → error + no comment.

**Source:** AC-06.1–AC-06.5.

---

## Backup / HTTPS

NFR-03 HTTPS та NFR-04 backup правильно залишені поза screen inventory.

Фінальний §8 містить:
- NFR-01 response time < 1 s;
- NFR-02 authentication required;
- NFR-03 HTTPS only;
- NFR-04 backup of all data.

У Screen Contracts вони правильно класифіковані як implementation-level verification, а не screen features.

---

## Deferred / Future

US-09 не повернута в UI plan.  
SR-10/SR-11 не повернуті в UI plan.

Це відповідає фінальному Release Scope.

---

# 4. Q-ID — what blocks generation vs what can wait

Попередні **Q-01…Q-10 закриті APPROVED D-02…D-14**. Нові Q нижче — лише наслідок критичної перевірки Screen Contracts.

## Blocking for a fully deterministic prototype/contract

### Q-REV-01
**Related defect:** DEF-01  
Успішний M-01 → S-03 є схваленим navigation behavior чи лише попереднім UX-рішенням?

**Status:** UNRESOLVED.  
**Impact:** блокує детермінований prototype flow після Create, але **не блокує генерацію самих frames**.

### Q-REV-02
**Related defect:** DEF-07  
Як має поводитися status control у S-03, коли current status = Closed і дозволених outgoing transitions немає?

**Status:** UNRESOLVED.  
**Impact:** блокує повністю детермінований status interaction prototype для Closed; не блокує screen frame.

### Q-REV-03
**Related defect:** DEF-06  
Як візуально представляються forbidden status destinations?

**Status:** UNRESOLVED.  
**Impact:** блокує деталізований prototype interaction, але не базовий screen generation.

---

## Can be postponed

### Q-REV-04
Точний empty-state copy для S-02-C.

### Q-REV-05
Точний error copy для порожнього Description.

### Q-REV-06
Точний error copy для failed comment save.

### Q-REV-07
Concrete widget type for Title/Description/Comment/Selectors.

### Q-REV-08
Exact visual label for Back / modal close.

Ці питання **не змінюють source requirements і не потребують нових product features**.

---

# 5. PLAN READY / NOT READY

## Recommendation: **NOT READY**

Не через screen inventory — він структурно відповідає погодженій моделі:

**S-01 → S-02-A/B/C ↔ M-01 → S-03**

і не вводить окремих Assignment/Priority/Status/Comments screens.

Причини статусу **NOT READY**:

1. **AC-07.3** не має повного implementation verification “next view”.
2. **AC-07.5** не має окремої перевірки invalid/out-of-list status value.
3. У T-05 navigation `M-01 → S-03` подано як source-backed, хоча такого D-рішення немає.
4. Поведінка status control для Closed та presentation forbidden destinations не визначена погодженим D-рішенням.

Після цих мінімальних виправлень recommendation може змінитися на READY. Остаточне рішення щодо readiness залишається за розробником.

---

# 6. Minimal corrections — without rewriting unaffected sections

### Change 1 — T-05 / M-01 exit
Було концептуально:  
`successful Create → S-03` як source-backed behavior.

Потрібно:  
помітити navigation як **PROPOSED D-ID / UNRESOLVED**, не як SOURCE.

### Change 2 — M-01 dismissal
Було:  
close → previous S-02 state як established rule.

Потрібно:  
залишити як **PROPOSED D-ID**, якщо команда хоче це зафіксувати; D-07 його не покриває.

### Change 3 — TEST-12…16
Додати окремий implementation test для **AC-07.3**:

`change status → leave S-03 → reopen same issue → verify persisted new status`.

### Change 4 — AC-07.5
Додати implementation test:

`attempt invalid status value outside {Open, In Progress, Resolved, Closed} → reject → no invalid state stored`.

Це не потребує нового UI control.

### Change 5 — TEST-04 / T-07
Залишити `Description required` як SOURCE.  
Будь-які твердження про конкретний error copy або обов’язкове залишення саме M-01 позначити як **PROPOSED D-ID**, а не SOURCE.

### Change 6 — S-03 C-14 / Closed
Не змінювати BR-08.1.  
Окремо позначити presentation/interaction of status control in Closed state як **UNRESOLVED / PROPOSED D-ID**.

### Change 7 — Traceability
У traceability:
- не використовувати US-01/AC як джерело для непогодженого navigation;
- додати AC-07.3 до нового persistence/reopen test;
- додати AC-07.5 до нового invalid-value implementation test.

---

# 7. Final critical-review result

**Coverage:** 31 із 33 numbered AC однозначно покриті; **2 AC мають неоднозначність verification** — AC-07.3 та AC-07.5.

**US-03:** усі чотири вихідні ненумеровані пункти покриті без вигаданих AC-ID.

**Permissions:** коректні, включно з Team Lead non-assignee ≠ status editor.

**Status model:** дозволені лише BR-08.1; Closed не має outgoing transitions; Resolved не прирівнюється до Closed.

**Important:** predicate точний; High+Resolved включено, Critical+Closed виключено, Not Set виключено.

**Create:** користувацькі поля обмежені Title + Description; автоматичні значення відокремлені.

**Comments:** author/time і failed-save/no-comment збережені.

**Scope:** Deferred/Future функції не відновлені.

**NFR:** HTTPS/backup не перетворені на screens.

**Source IDs:** оригінальні US/AC/BR/NFR не перейменовані; для US-03 нових source IDs не вигадано.

**Readiness:** **NOT READY** до мінімальних виправлень DEF-01…DEF-07.