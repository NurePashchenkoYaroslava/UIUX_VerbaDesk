# VerbaDesk — UX Architecture Input Pack v1.0

## 1. Architectural boundary

### Release MVP

**Included:** US-01–US-08.  
**Deferred:** US-09.  
**Future:** SR-10, SR-11.

### Approved product surfaces

| Type | ID | Name |
|---|---|---|
| Screen boundary / placeholder | **S-01** | Authentication boundary |
| Screen | **S-02** | Issues List |
| Screen states | **S-02-A** | All |
| Screen state | **S-02-B** | Important |
| Screen state | **S-02-C** | Empty |
| Modal / overlay | **M-01** | Create Issue modal |
| Screen | **S-03** | Issue Details |

**Approved:** one list with `All / Important` tabs and Create modal; assignment, priority, status and comments are controls/states inside S-03, not separate screens. **Source:** D-01, D-06.  
Authentication is represented only as a boundary/placeholder; demonstration starts already authenticated at S-02. **Source:** D-02.

---

# 2. Screen / state inventory

## S-01 — Authentication boundary

| Field | Definition |
|---|---|
| **Screen/State ID** | S-01 |
| **Name** | Authentication boundary |
| **Type** | Boundary / placeholder, not a designed login screen |
| **Purpose** | Позначити обов’язкову межу authentication перед доступом до MVP |
| **Actors** | Authenticated Team Member: Translator / Manager |
| **Entry** | Before access to S-02; demonstration starts after authentication |
| **Visible data** | Лише межа authentication; конкретний login UI не визначений |
| **Inputs** | Не проєктуються |
| **Actions** | Не проєктуються |
| **Role/state conditions** | Access requires authenticated team member |
| **Errors/empty states** | Не визначаються в UI Pack |
| **Exit** | Authenticated → S-02 |
| **Source/D-ID** | §3 / BR-01; §1 NFR-02; **D-02** |

BR-01 визначає доступ лише для authenticated team members, а D-02 прямо обмежує S-01 лише authentication boundary. 
---

## S-02-A — Issues List / All

| Field | Definition |
|---|---|
| **Screen/State ID** | S-02-A |
| **Name** | Issues List — All |
| **Type** | Screen state of S-02 |
| **Purpose** | Перегляд усіх зареєстрованих заявок |
| **Actors** | Translator / Manager |
| **Entry** | S-01 after authentication; S-03 after returning with previous list state |
| **Visible data** | Для кожної заявки: ID, Title, Status, Priority, Assignee if assigned |
| **Inputs** | Вибір issue; перемикання `All / Important`; відкриття Create modal |
| **Actions** | Open issue → S-03; select Important → S-02-B; Create → M-01 |
| **Role/state conditions** | Авторизований team member; list tab = All |
| **Errors/empty states** | Якщо заявок немає → S-02-C |
| **Exit** | S-03; S-02-B; M-01 |
| **Source/D-ID** | §5 / US-02 / AC-02.1–AC-02.3; **D-01, D-07** |

**Source:** список має показувати щонайменше ID, Title, Status, Priority, Assignee if assigned.

---

## S-02-B — Issues List / Important

| Field | Definition |
|---|---|
| **Screen/State ID** | S-02-B |
| **Name** | Issues List — Important |
| **Type** | Screen state of S-02 |
| **Purpose** | Швидкий перегляд важливих незакритих заявок |
| **Actors** | Translator / Manager |
| **Entry** | Select `Important` from S-02 |
| **Visible data** | Issue rows with at least ID, Title, Status, Priority, Assignee if assigned |
| **Inputs** | Перемикання `All`; вибір issue |
| **Actions** | Select issue → S-03; select All → S-02-A |
| **Role/state conditions** | Important if Priority = High or Critical and status ≠ Closed |
| **Errors/empty states** | Відсутність matching issues означає порожній результат; конкретний текст не визначений джерелом |
| **Exit** | S-02-A; S-03 |
| **Source/D-ID** | §3 / BR-09, BR-09.1; §5 / US-08 / AC-08.1–AC-08.4; **D-01, D-10** |

За D-10 назва “Open Issues” трактує всі заявки зі статусом ≠ Closed, включно з Resolved. Це узгоджується з BR-09.1 та AC-08.1–AC-08.4. 
---

## S-02-C — Issues List / Empty

| Field | Definition |
|---|---|
| **Screen/State ID** | S-02-C |
| **Name** | Issues List — Empty |
| **Type** | Screen state of S-02 |
| **Purpose** | Показати відсутність заявок |
| **Actors** | Translator / Manager |
| **Entry** | S-02-A або S-02-B, коли відповідний список не містить заявок |
| **Visible data** | Повідомлення про відсутність заявок |
| **Inputs** | `All / Important`; Create |
| **Actions** | Switch tab; open M-01 |
| **Role/state conditions** | Відповідний list state не має записів |
| **Errors/empty states** | Це сам empty state; за D-11 не використовується порожня таблиця |
| **Exit** | S-02-A/B; M-01 |
| **Source/D-ID** | §5 / US-02 / AC-02.5; **D-11** |

AC-02.5 допускає empty list/message, а D-11 фіксує конкретно повідомлення про відсутність заявок.

---

## M-01 — Create Issue

| Field | Definition |
|---|---|
| **Screen/State ID** | M-01 |
| **Name** | Create Issue modal |
| **Type** | Modal / overlay over S-02 |
| **Purpose** | Створення нової заявки |
| **Actors** | Translator / Manager |
| **Entry** | Create action from S-02-A/S-02-B/S-02-C |
| **Visible data** | Title; Description; Create action; modal context |
| **Inputs** | Title; Description |
| **Actions** | Create; close/cancel modal |
| **Role/state conditions** | Будь-який authenticated team member може створити issue; Title і Description required |
| **Errors/empty states** | Empty Title → issue not created + `Title is required`; empty Description must block creation, exact error text not defined |
| **Exit** | Successful Create → S-03; invalid submission → remain in M-01; close → previous S-02 state |
| **Source/D-ID** | §5 / US-01 / AC-01.1–AC-01.5 + scenarios; §3 / BR-02–BR-04; **D-04, D-07, D-09** |

Description приймає будь-який текст або посилання без format validation за D-09. Title і Description є обов’язковими за AC-01.2–AC-01.3.

Після успішного створення issue отримує ID, `status = Open`, `created_at`, `author`; priority = `Not Set`; assignee за D-04 відсутній.

---

## S-03 — Issue Details

| Field | Definition |
|---|---|
| **Screen/State ID** | S-03 |
| **Name** | Issue Details |
| **Type** | Screen |
| **Purpose** | Перегляд і виконання всіх дозволених дій над конкретною заявкою в одному контексті |
| **Actors** | Translator / Manager |
| **Entry** | Select issue from S-02; successful create from M-01 |
| **Visible data** | ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at; comments |
| **Inputs** | Comment text; assignee control; priority control; status control |
| **Actions** | Add Comment; Apply assignee; Apply priority; Apply status; navigate back to previous list state |
| **Role/state conditions** | Manager controls assignment/priority; Current Assignee controls status; unauthorized role-dependent controls disabled, not hidden |
| **Errors/empty states** | Comment empty; comment save error; issue may have no assignee; `Not Set` priority; invalid status transition unavailable/disabled |
| **Exit** | Back → previous S-02 state |
| **Source/D-ID** | §5 / US-02, US-03, US-04, US-05, US-06, US-07; §3 / BR-04–BR-09.1; **D-03, D-04, D-05, D-06, D-07, D-14** |

Деталі заявки мають містити набір із AC-02.4.

---

# 3. Component states and service annotations

Ці елементи **не є окремими продуктовими екранами**.

## Component states

| ID | Type | Component state | Behaviour | Source/D-ID |
|---|---|---|---|---|
| C-01 | Component state | Priority = Not Set | Явно показує `Not Set`; доступний вибір лише Low / Normal / High / Critical; reset не додається | §3 / BR-04–BR-05; **D-03** |
| C-02 | Component state | Role-dependent control disabled | Заборонений control залишається видимим, але disabled | §5 / US-03, US-04, US-07 / role AC; **D-05** |
| C-03 | Component state | Apply interaction | Assignment, priority, status застосовуються кнопкою `Apply` біля відповідного control; успіх залишає S-03 | **D-06** |
| C-04 | Component state | Comment success | Після успішного збереження comment відображається для issue разом з author/date-time | §5 / US-06 / AC-06.2–AC-06.4 |
| C-05 | Component state | Comment save error | Помилка відображається; comment не створюється | §5 / US-06 / AC-06.5 |
| C-06 | Component state | Status control | Дозволяються тільки значення Open / In Progress / Resolved / Closed і переходи BR-08.1 | §3 / BR-07, BR-08.1; **D-05, D-06** |

## Service / flow annotations

| ID | Type | Annotation | Meaning | Source/D-ID |
|---|---|---|---|---|
| A-01 | Service annotation | Previous List State | При переході S-02 → S-03 зберігається попередня вкладка All/Important; Back повертає саме туди | **D-07** |
| A-02 | Service annotation | Demo data | У макеті — dummy data; у прототипі — predetermined outcomes | **D-08** |
| A-03 | Service annotation | Authenticated start | Демонстрація не проходить реальний login; стартує після authentication boundary | **D-02, D-08** |
| A-04 | Service annotation | Date-time format | Стандартний формат, наприклад `DD.MM.YYYY HH:MM`; окремих timezone settings у макеті немає | **D-14** |

---

# 4. Transition matrix

| T-ID | From | Actor | Trigger | Preconditions | To | Visible result | Requirement/D-ID |
|---|---|---|---|---|---|---|---|
| T-01 | S-01 | Authenticated member | Authentication boundary passed | User authenticated | S-02-A | Issues List / All | BR-01, NFR-02 / **D-02** |
| T-02 | S-02-A | Team Member | Select `Important` | Tab available | S-02-B | Important list | US-08 / **D-01, D-10** |
| T-03 | S-02-B | Team Member | Select `All` | Tab available | S-02-A | All list | **D-01** |
| T-04 | S-02-A/B/C | Team Member | Click Create | Authenticated | M-01 | Create modal opens | US-01 / **D-01** |
| T-05 | M-01 | Team Member | Create | Title + Description valid | S-03 | New issue details; Open; Not Set; no assignee | US-01 / AC-01.4–AC-01.5; **D-03, D-04** |
| T-06 | M-01 | Team Member | Create | Title empty | — | Error `Title is required`; remain M-01 | US-01 / scenario AC-02 |
| T-07 | M-01 | Team Member | Create | Description empty | — | Issue not created; remain M-01 | US-01 / AC-01.3 |
| T-08 | S-02-A/B | Team Member | Select issue | Issue exists | S-03 | Issue Details | US-02 / AC-02.3 |
| T-09 | S-03 | Manager | Select assignee + Apply | Selected assignee is team member | S-03 | Current assignee displayed | US-03; **D-04, D-06** |
| T-10 | S-03 | Manager | Change priority + Apply | Value = Low/Normal/High/Critical | S-03 | New priority displayed | US-04; BR-05–BR-06; **D-03, D-06** |
| T-11 | S-03 | Current Assignee | Open → In Progress + Apply | Current status Open | S-03 | Status = In Progress | BR-08.1; US-07; **D-06** |
| T-12 | S-03 | Current Assignee | In Progress → Resolved + Apply | Current status In Progress | S-03 | Status = Resolved | BR-08.1; US-07; **D-06** |
| T-13 | S-03 | Current Assignee | Resolved → Closed + Apply | Current status Resolved | S-03 | Status = Closed; no longer Important | BR-08.1; AC-08.4; **D-06** |
| T-14 | S-03 | Current Assignee | Resolved → In Progress + Apply | Current status Resolved | S-03 | Work reopened; status = In Progress | BR-08.1; **D-06** |
| T-15 | S-03 | Current Assignee | Open → Closed + Apply | Cancellation before processing | S-03 | Status = Closed | BR-08.1; **D-12** |
| T-16 | S-03 | Team Member | Add valid comment | Comment text non-empty; save succeeds | S-03 | Comment displayed with author/date-time | US-06 / AC-06.1–AC-06.4 |
| T-17 | S-03 | Team Member | Save comment | Save fails | — | Error; no comment created | US-06 / AC-06.5 |
| T-18 | S-03 | Team Member | Back | Previous list state stored | S-02-A or S-02-B | Previous tab restored | **D-07** |
| T-19 | S-02-A/B | Team Member | List has no records | No matching issues | S-02-C | Clear no-issues message | US-02 / AC-02.5; **D-11** |
| T-20 | S-03 | non-Manager | Change priority / Apply | User is Translator | — | Control remains visible but disabled | US-04 / AC-04.2; **D-05** |
| T-21 | S-03 | non-Manager | Assign / Apply | User is not Manager | — | Control remains visible but disabled | US-03; **D-05** |
| T-22 | S-03 | non-Assignee | Change status / Apply | User is not Current Assignee | — | Control remains visible but disabled; no status change | US-07 / AC-07.4; **D-05** |
| T-23 | S-03 | Authorized actor | Unsupported status transition | Transition not allowed by BR-08.1 | — | No transition; status remains unchanged | BR-08.1; **D-05, D-06** |

**Правило:** рядки T-06, T-07, T-17, T-20–T-23 не створюють navigation transition. Вони описують відмову/відсутність переходу або component state.

---

# 5. Status transition model

Дозволений workflow у межах S-03:

```text
Open
 ├──→ In Progress ───→ Resolved ───→ Closed
 │                         │
 │                         └──────→ In Progress
 │
 └──→ Closed   [cancellation before processing]
```

**Інші переходи не дозволені.** `Resolved` не прирівнюється до `Closed`: до Closed issue залишається активною та може входити до Important Issues за High/Critical.

---

# 6. Textual user flows

## UF-01 — Create issue

`S-02-A/B/C → M-01 → S-03`

Користувач відкриває Create → вводить Title + Description → Create → система створює issue зі статусом `Open`, priority `Not Set`, без assignee → відкривається S-03.

**Invalid:** порожній Title → `M-01 → M-01`, без створення issue, показ `Title is required`.

**Source:** US-01 / AC-01.1–AC-01.5; **D-03, D-04, D-09**.

---

## UF-02 — View issue

`S-02-A/B → S-03 → S-02-A/B`

Користувач обирає issue зі списку → S-03 показує всі передбачені деталі → Back → повернення до тієї самої вкладки, з якої issue було відкрито.

**Source:** US-02 / AC-02.1–AC-02.5; **D-07**.

---

## UF-03 — Manager assigns issue and sets priority

`S-02-A/B → S-03 → S-03`

Manager відкриває issue → обирає одного team-member assignee → `Apply` → assignee відображається → змінює priority на Low/Normal/High/Critical → `Apply` → нове priority відображається.

Після створення issue до призначення assignee відсутній.

**Source:** US-03, US-04; BR-04–BR-06; **D-03, D-04, D-06**.

---

## UF-04 — Update status through allowed workflow

`S-02-A/B → S-03 → S-03`

Current Assignee:
`Open → In Progress → Resolved → Closed`.

Кожна зміна виконується через відповідний control + `Apply`.

Недозволений перехід не створює transition.

**Source:** US-07 / AC-07.1–AC-07.5; BR-07, BR-08.1; **D-05, D-06**.

---

## UF-05 — Reopen Resolved issue

`S-03 (Resolved) → S-03 (In Progress)`

Current Assignee вибирає `In Progress` → `Apply` → issue залишається у S-03 зі статусом `In Progress`.

**Source:** BR-08.1; BR-09.1; **D-06**.

---

## UF-06 — Comment success / error

### Success
`S-03 → S-03`

Team Member вводить непорожній comment → Save → comment відображається разом з author та creation date/time.

### Error
`S-03 → S-03`

Save fails → error state component → comment не додається.

**Source:** US-06 / AC-06.1–AC-06.5.

---

## UF-07 — View Important issues

`S-02-A → S-02-B → S-03 → S-02-B`

Team Member обирає `Important` → бачить issues із `High/Critical` та status ≠ Closed → відкриває issue → Back повертає саме в Important.

`Closed` не відображається незалежно від priority.

**Source:** US-08 / AC-08.1–AC-08.4; BR-09.1; **D-01, D-07, D-10**. 
---

## UF-08 — Unauthorized action

`S-03 → S-03`

Translator бачить assignment/priority controls, але вони disabled → action неможлива.

Не-Assignee бачить status control disabled → не може виконати зміну.

**Недозволена дія не створює transition.**

**Source:** US-03, US-04, US-07; **D-05**.

---

## UF-09 — Empty list

`S-02-A/B → S-02-C`

Якщо відповідний список не має заявок → показується зрозуміле повідомлення про відсутність заявок, а не порожня таблиця.

**Source:** US-02 / AC-02.5; **D-11**.

---

# 7. Three things NOT to add

Навіть якщо вони типові для issue trackers, у Release MVP вони не мають source/approved basis.

### 1. Окрема сторінка Assignment
Assignment — control усередині **S-03**, а не самостійний screen.

### 2. Видалення issue
BR-10 прямо визначає, що заявки **не видаляються**.

### 3. User-facing change history / activity log
BR-11 прямо визначає, що окрема користувацька історія змін у першій версії не передбачається.

Так само не додаються attachments, AI categorization, AI priority recommendation, billing, customer portal, mobile application або full project-management functions: вони не належать Release MVP. 
---

# 8. Figma demonstration frames ≠ product screens

## Product screens / states

Мінімальна продуктова модель:

**4 approved surface IDs**
- S-01
- S-02
- M-01
- S-03

При цьому S-02 має 3 стани: A/B/C.

Отже, це **не 7 окремих продукт-сторінок**.

## Recommended demonstration frames

Для повної демонстрації погодженого flow достатньо **8 Figma frames**:

| Frame | Represents |
|---|---|
| F-01 | S-01 authentication boundary placeholder |
| F-02 | S-02-A All |
| F-03 | S-02-B Important |
| F-04 | S-02-C Empty |
| F-05 | M-01 Create — normal/valid state |
| F-06 | M-01 Create — validation error state |
| F-07 | S-03 — Manager view with assignment/priority/status controls |
| F-08 | S-03 — comment error / role-disabled state demonstration |

`F-08` може бути побудований як той самий S-03 з component states/variants; він не створює нового продуктового screen ID.

Для status workflow, comment success, reopen та Back не потрібні окремі продуктові screens: вони можуть бути продемонстровані як prototype interaction/state changes у S-03.

**Source/D-ID:** D-01, D-02, D-05, D-06, D-07, D-08.

---

# 9. Minimum Screen Inventory

| ID | Classification | Name | Product-level role |
|---|---|---|---|
| **S-01** | Boundary / placeholder | Authentication boundary | Позначає auth boundary; без login design |
| **S-02-A** | Screen state | Issues List — All | Основний список |
| **S-02-B** | Screen state | Issues List — Important | Important subset |
| **S-02-C** | Screen state | Issues List — Empty | Empty state |
| **M-01** | Modal / overlay | Create Issue | Створення issue |
| **S-03** | Screen | Issue Details | Деталі + assignment + priority + status + comments |

**Мінімум:** 3 фактичні content screens (`S-02`, `S-03` + boundary `S-01`) і 1 modal; S-02-A/B/C — стани одного screen.

---

# 10. Unresolved decisions

З попереднього Input Pack питання **Q-01–Q-10 закриті відповідними APPROVED D-02–D-14**.

Для поточного архітектурного плану все ще **не задано APPROVED-рішеннями**:

1. точний текст повідомлення для порожнього Important list;
2. точний текст помилки failed comment save;
3. точний текст помилки для порожнього Description;
4. порядок/сортування issues у списку;
5. точний візуальний формат role-disabled controls;
6. точний набір демонстраційних dummy values.

Ці пункти **не перетворюються на requirements** і не блокують базову структуру S-01/S-02/M-01/S-03.

---

# 11. Criteria not fully covered by the plan

## Runtime / implementation criteria

Макет та прототип не доводять:

- **NFR-01** — Response time < 1 s;
- **NFR-03** — HTTPS only;
- **NFR-04** — Backup of all data;
- фактичну реалізацію **NFR-02 Authentication required**;
- фактичне persistence behavior для AC-01.4, AC-01.5;
- фактичне persistence behavior для AC-07.3;
- фактичну відсутність створення comment після failed save за AC-06.5.

## Частково не визначені source details

- AC-01.3 вимагає обов’язковий Description, але окремий текст помилки не задано;
- AC-06.5 вимагає error, але конкретне повідомлення не задано;
- AC-02.5 не задає exact copy empty message;
- US-03 не надає numeric IDs для своїх чотирьох Acceptance Criteria, тому нові AC-ID для них не створювалися.

## Scope criteria intentionally absent

Не покриваються як Release MVP:
- US-09;
- SR-10;
- SR-11.

---

# 12. Architectural conclusion

Поточна мінімальна модель без додаткових продуктових екранів:

**S-01 → S-02 (A/B/C) ↔ M-01 → S-03 ↔ S-02 (previous state)**

Уся робоча координація issue — **assignee, priority, status, comments** — залишається всередині **S-03**. Заборонені дії не створюють navigation transitions; role-dependent controls залишаються видимими disabled. **D-01–D-14 використовуються як APPROVED design decisions; жодних інших рішень цей план не оголошує погодженими.**