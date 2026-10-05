# VerbaDesk — UX/UI Input Pack v1.0

## 0. Source and status

**Source file:** `Translation_Spec.docx`.

У файлі доступні розділи 0–9. Розділ 9 «UX/UI & Interaction Design» ще не містить макетів або прототипів, тому цей Pack є вхідними даними для їх подальшого проєктування.

**Final Release Scope:**  
- **Release MVP:** US-01–US-08
- **Deferred:** US-09
- **Future:** SR-10, SR-11.

---

# 1. SOURCE REQUIREMENTS

## 1.1 Product purpose and boundaries

VerbaDesk — внутрішня web-система для невеликого бюро перекладів, призначена для реєстрації, відстеження та координації виконання запитів на переклад.

**Primary users / actors:**
- Translator (Team Member)
- Manager (Team Lead)

У першій версії передбачено 3 заздалегідь визначені користувачі: 2 Translator та 1 Manager. Лише автентифіковані члени команди мають доступ до системи; зовнішні клієнти користувачами не є.

---

## 1.2 Included US

### US-01 — Create Issue
**Actor:** Translator / Manager  
**Мета:** створити заявку з title та description.

**AC-01.1** Користувач може ввести: Title, Description.  
**AC-01.2** Title є обов'язковим.  
**AC-01.3** Description є обов'язковим.  
**AC-01.4** Після створення заявка отримує: ID, status = Open, created_at, author.  
**AC-01.5** Після створення заявки її priority не встановлено.

У сценарії створення користувач є authenticated, працює на `"New Issue"` page, натискає `"Create"`; нова заявка має status `Open`, unique ID і поточного user як author. При порожньому Title заявка не створюється та відображається `"Title is required"`.

**Source:** §5 / US-01 / AC-01.1–AC-01.5 та сценарії.

### US-02 — View Issues
**Actor:** Team Member  
**Мета:** переглядати зареєстровані заявки та їх деталі.

**AC-02.1** Авторизований член команди може переглянути список зареєстрованих заявок.  
**AC-02.2** У списку для кожної заявки відображаються щонайменше: ID, Title, Status, Priority, Assignee, якщо він призначений.  
**AC-02.3** Користувач може вибрати заявку зі списку та відкрити сторінку її деталей.  
**AC-02.4** На сторінці деталей відображаються: ID, Title, Description, Status, Priority, Assignee (якщо призначений), Author, Created at.  
**AC-02.5** Якщо заявок немає, система відображає порожній список або повідомлення про відсутність.

**Source:** §5 / US-02 / AC-02.1–AC-02.5.

### US-03 — Assign Issue to the responsible team member
**Actor:** Manager (Team Lead)  
**Мета:** призначити заявку відповідальному члену команди.

Acceptance Criteria, задані без numeric IDs:
- можна вибрати одного assignee;
- assignee повинен бути членом команди;
- призначати заявку може лише Manager;
- поточний assignee відображається у заявці.

**Source:** §5 / US-03 / Acceptance Criteria 03.

### US-04 — Set Issue Priority
**Actor:** Manager (Team Lead)  
**Мета:** встановлювати priority заявки.

**АС-04.1** Допустимі значення встановленого Priority: Low, Normal, High, Critical.  
**АС-04.2** Встановлювати та змінювати priority може лише Manager.

**Source:** §5 / US-04 / АС-04.1–АС-04.2.

### US-05 — Track Issue Progress
**Actor:** Team Member  
**Мета:** бачити поточний status заявки.

**АС-05.1** Поточний status відображається для кожної заявки.  
**AC-05.2** Status відображається на сторінці заявки.  
**AC-05.3** Status відображається у списку.

**Source:** §5 / US-05 / АС-05.1, AC-05.2, AC-05.3.

### US-06 — Add Comment
**Actor:** Team Member  
**Мета:** додавати коментарі до заявки.

**AC-06.1** Comment text cannot be empty.  
**AC-06.2** Added comment is stored and shown for the issue.  
**AC-06.3** Comment author is stored and displayed.  
**AC-06.4** Creation date/time is stored and displayed.  
**AC-06.5** Failed save does not create a comment and produces an error.

**Source:** §5 / US-06 / AC-06.1–AC-06.5.

### US-07 — Update Issue Status
**Actor:** член команди, відповідальний за заявку.

**АС-07.1** Нова заявка створюється зі статусом Open.  
**АС-07.2** Допустимі статуси: Open, In Progress, Resolved, Closed.  
**АС-07.3** Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді.  
**АС-07.4** Користувач, який не є виконавцем заявки, не може змінити її статус.  
**АС-07.5** Система не дозволяє встановити значення статусу, якого немає у визначеному переліку.

**Source:** §5 / US-07 / АС-07.1–АС-07.5.

### US-08 — View Important Open Issues
**Actor:** Team Member  
**Мета:** швидко бачити важливі відкриті заявки.

**AC-08.1** `priority = High` + status ≠ Closed → відображається в Important Issues.  
**AC-08.2** `priority = Critical` + status ≠ Closed → відображається в Important Issues.  
**AC-08.3** `priority = Low` або `Normal` → не відображається в Important Issues.  
**AC-08.4** `status = Closed` → не відображається незалежно від priority.

**Source:** §5 / US-08 / AC-08.1–AC-08.4.

---

## 1.3 SOURCE Business Rules relevant to UI

**BR-01 — Authorized users:** лише автентифіковані члени команди; у v1 — 2 Translator + 1 Manager.

**BR-02 — Issue creation:** будь-який автентифікований член команди може створити заявку.

**BR-03 — Initial issue status:** `status = Open`.

**BR-04 — Initial issue priority:** `priority = Not Set`.

**BR-05 — Allowed priority values:** `Low`, `Normal`, `High`, `Critical`; до встановлення допускається `Not Set` / пусте значення.

**BR-06 — Priority management:** тільки Manager може встановлювати/змінювати priority.

**BR-07 — Allowed status values:** `Open`, `In Progress`, `Resolved`, `Closed`.

**BR-08 — Status management:** status змінює відповідальний за заявку член команди.

**BR-08.1 — Allowed transitions:**
- Open → In Progress
- In Progress → Resolved
- Resolved → Closed
- Resolved → In Progress
- Open → Closed при скасуванні клієнтом до початку обробки
- інші переходи не дозволені.

**BR-09 — Important issue:** priority `High` або `Critical`.

**BR-09.1:** Resolved — вирішена, але ще не закрита; до Closed заявка залишається активною та може бути в Important Issues відповідно до priority.

**BR-10 — Issue deletion:** заявки не видаляються.

**BR-11 — Change history:** окрема користувацька історія змін у першій версії не передбачається.

**BR-12 — AI recommendations:** AI може лише рекомендувати category/priority і не змінює їх автоматично без дії користувача. Це правило не реалізується в Release MVP, оскільки SR-10/SR-11 — Future.

---

## 1.4 SOURCE NFR

**NFR-01:** Response time < 1 s.  
**NFR-02:** Authentication required.  
**NFR-03:** HTTPS only.  
**NFR-04:** Backup of all data у фінальному Release Scope.

**Source:** §8 / Updated NFRs.

NFR-01, NFR-03, NFR-04 не можуть бути доведені статичним макетом. NFR-02 визначає обов’язковість authentication, але не визначає конкретний спосіб.

---

## 1.5 SOURCE exclusions

**Deferred:**
- US-09 — Attach files to an issue.

AC-09.1–AC-09.6 щодо storage, limits, malware scanning та integrity відкладені.

**Future:**
- SR-10 — Automatic issue categorization.
- SR-11 — Automatic Priority recommendation.

**Explicitly Out of Scope:**
Customer support portal, Billing, CI/CD, Time tracking, Full project management, Mobile application та інші напрями, прямо зазначені в §2.

---

# 2. APPROVED DESIGN DECISIONS

Лише рішення зі статусом **APPROVED** із наданого Decision Log.

### D-01 — APPROVED
**Desktop, grayscale, low-fidelity.**  
Один список із вкладками **All / Important**; **Create modal**.

### D-02 — APPROVED
**S-01** — лише межа authentication. Демонстрація стартує з уже автентифікованого **S-02**.  
Закриває **Q-09**.

### D-03 — APPROVED
Відсутність priority показувати як **Not Set**. Вибір лише **Low / Normal / High / Critical**. Reset не додавати.  
Закриває **Q-05**.

### D-04 — APPROVED
Після створення **assignee відсутній**. Manager може перепризначати заявку.  
Закриває **Q-06**.

### D-05 — APPROVED
Заборонені role-dependent controls показувати **disabled**, не приховувати.  
Закриває **Q-08**.

### D-06 — APPROVED
Для зміни **assignee / priority / status** використовується **Apply** біля відповідного контролу. Успішна зміна залишає користувача у **S-03**.

### D-07 — APPROVED
Повернення з деталей зберігає попередню вкладку списку.

### D-08 — APPROVED
Макет використовує фіктивні дані, прототип — наперед задані результати; справжні login та БД не реалізуються.

### D-09 — APPROVED
**Description** приймає будь-який текст або посилання без окремої UI-валідації формату.  
Закриває **Q-01**.

### D-10 — APPROVED
Назва **Open Issues** у US-08 означає всі заявки зі статусом, відмінним від **Closed**, включно з **Resolved**.  
Закриває **Q-02**.

### D-11 — APPROVED
За відсутності заявок показується зрозуміле повідомлення про їх відсутність, а не порожня таблиця.  
Закриває **Q-07**.

### D-12 — APPROVED
Перехід **Open → Closed** при скасуванні виконує призначений перекладач (**Current Assignee**).  
Закриває **Q-03**.

### D-13 — APPROVED
Оскільки US-09 Deferred, **NFR-04** для цього релізу застосовується лише до текстових даних заявок. Інтерфейс attachments не малюється.  
Закриває **Q-04**.

### D-14 — APPROVED
Дата/час відображається у стандартному форматі, наприклад **DD.MM.YYYY HH:MM**; окремі timezone settings у макеті не передбачаються.  
Закриває **Q-10**.

---

# 3. UNRESOLVED

Раніше зафіксовані питання:

- **Q-01** — закрито D-09.
- **Q-02** — закрито D-10.
- **Q-03** — закрито D-12.
- **Q-04** — закрито D-13.
- **Q-05** — закрито D-03.
- **Q-06** — закрито D-04.
- **Q-07** — закрито D-11.
- **Q-08** — закрито D-05.
- **Q-09** — закрито D-02.
- **Q-10** — закрито D-14.

**Інших окремо погоджених рішень у Decision Log не заявлено.** Нові рішення до Pack не додаються.

**Поточний статус блокування плану екранів:** відсутні UNRESOLVED-питання з наданого Decision Log, які блокують початок планування екранів.

---

# 4. DESIGN BOUNDARY

Для подальшого UX/UI проєктування дозволено спиратися на:

1. SOURCE requirements із §1 цього Pack.
2. APPROVED decisions D-01–D-14 із §2.

Не слід трактувати як затверджені:
- будь-які попередні PROPOSED-рішення;
- способи authentication, не вказані в D-02;
- будь-які нові поля, ролі, екрани, функції або механізми збереження;
- функціональність US-09, SR-10, SR-11.

---

# 5. SOURCE CHECK

### Included US → AC → Pack

**US-01** → AC-01.1–AC-01.5 + обидва наведені сценарії → §1.2 US-01  
**US-02** → AC-02.1–AC-02.5 → §1.2 US-02  
**US-03** → усі 4 ненумеровані Acceptance Criteria → §1.2 US-03  
**US-04** → АС-04.1–АС-04.2 → §1.2 US-04  
**US-05** → АС-05.1, AC-05.2, AC-05.3 → §1.2 US-05  
**US-06** → AC-06.1–AC-06.5 → §1.2 US-06  
**US-07** → АС-07.1–АС-07.5 → §1.2 US-07  
**US-08** → AC-08.1–AC-08.4 → §1.2 US-08

### Excluded requirements → reason

**US-09** → Deferred у фінальному Release Scope.  
**SR-10** → Future.  
**SR-11** → Future.  
**Customer support portal / Billing / CI/CD / Time tracking / Full project management / Mobile application** → Explicitly Out of Scope.

### Unresolved questions

За наданим Decision Log **Q-01–Q-10 закриті рішеннями D-02–D-14**; окремих невирішених питань із цього Decision Log не залишилось.

**Pack v1.0 не оголошується погодженим загалом.**