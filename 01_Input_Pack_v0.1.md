# UX/UI Design Input Pack — VerbaDesk

## 0. Source Basis and Document Status

**Прочитаний файл:** `Translation_Spec.docx`

**Доступні розділи:**  
0. Початковий запит замовника — Customer Request  
1. Discovery / Problem Analysis  
2. Product Vision + Initial Product Scope  
3. Stakeholder Needs & Stakeholder Requirements  
4. Prioritization of Needs + Candidate MVP  
5. Detailed Functional Requirements Specification / User Stories + Acceptance Criteria  
6. Quality Attributes + Security Requirements  
7. Feasibility, Estimation & Risk Assessment  
8. MVP / Release Scope  
9. UX/UI & Interaction Design — розділ ще не заповнений макетами та прототипами. 
**Статус Pack:** не погоджений; містить зафіксовані вимоги, похідні висновки та окремо невирішені питання.

**Правило визначення релізу:** для цього Pack використовується остаточний **Release Scope із розділу 8**, а не Candidate MVP із розділу 4. У фінальному scope вказано: **Release MVP: US-01..US-08; Deferred: US-09; Future: SR-10, SR-11.**

---

# 1. Product Purpose

VerbaDesk — внутрішня web-система для невеликого бюро перекладів, призначена для реєстрації, відстеження та координації виконання запитів на переклад.

**Actors / primary users першого релізу:**  
- **Translator (Team Member)** — член команди / відповідальний перекладач;
- **Manager (Team Lead)** — керівник команди.

У першій версії передбачено три заздалегідь задані члени команди: **2 Translator + 1 Manager**. Користуватися системою можуть лише автентифіковані члени команди; зовнішні клієнти не є користувачами системи. 
**Межі першого релізу:** US-01–US-08; US-09 відкладено; SR-10 та SR-11 визначені як Future. Система є web application; mobile application, зовнішні трекери/CRM та інші явно вказані позарелізні напрями не входять до першого релізу. 
---

# 2. Scope

## 2.1 Included — Release MVP

| US | Назва | Actor |
|---|---|---|
| US-01 | Create Issue | Translator / Manager |
| US-02 | View Issues | Team Member |
| US-03 | Assign Issue to the responsible team member | Manager |
| US-04 | Set Issue Priority | Manager |
| US-05 | Track Issue Progress | Team Member |
| US-06 | Add Comment | Team Member |
| US-07 | Update Issue Status | відповідальний за заявку член команди / перекладач |
| US-08 | View Important Open Issues | Team Member |

Фінальний Release Scope прямо визначає US-01..US-08 як Release MVP.

## 2.2 Deferred

**US-09 — Attach files to an issue.**

US-09 має статус **DEFERRED**. Пов’язані AC-09.1–AC-09.6 стосуються вимог до file storage, лімітів розміру, антивірусного сканування та цілісності; їх виконання відкладене до майбутніх релізів.

Причина перенесення: US-09 має значно вищу складність та потребує file storage, file size limits, malware scanning і backup; у feasibility assessment рекомендовано винести US-09 за межі першого релізу.

## 2.3 Future

- **SR-10 — Automatic issue categorization**
- **SR-11 — Automatic Priority recommendation**

Обидві вимоги у фінальному Release Scope визначені як **Future**. 
AI-функціональність не входить до обов’язкової функціональності першого релізу; за припущенням A-08 AI має мати рекомендаційний характер і не повинен автоматично змінювати category або priority. 
## 2.4 Explicitly Out of Scope

У специфікації прямо названі такі напрями як **Out of Scope на поточному етапі**:

- Customer support portal;
- Billing;
- Continuous Integration / Continuous Delivery;
- Time tracking;
- Full project management;
- Mobile application;
- інші неназвані у списку напрямки (`etc.`).

Також встановлено, що користувачами системи є лише члени внутрішньої команди, а зовнішні клієнти користувачами не є; у першій версії не планується інтеграція із зовнішніми трекерами чи CRM.

**Не слід ототожнювати Deferred, Future та Out of Scope:** US-09 є Deferred; SR-10/SR-11 — Future; названі вище напрями — Explicitly Out of Scope.

---

# 3. Included User Stories and Acceptance Criteria

## US-01 — Create Issue

**Actor:** Translator / Manager

**Мета:** створити заявку із title та description, щоб зафіксувати translation task і мати можливість її відстежувати.

**Source:** §5 / US-01.

### Acceptance Criteria

**AC-01.1** Користувач може ввести: `Title`, `Description`.  
**AC-01.2** `Title` є обов'язковим.  
**AC-01.3** `Description` є обов'язковим.  
**AC-01.4** Після створення заявка отримує: `ID`, `status = Open`, `created_at`, `author`.  
**AC-01.5** Після створення заявки її `priority` не встановлено.  

**Source:** §5 / US-01 / AC-01.1–AC-01.5.

### Сценарії, наведені в специфікації

**Сценарій для АС-01:**  
Given the user is authenticated and is on the `"New Issue"` page;  
When the user enters the title `"Translation of Contract"` and description `"Translate to EN"` and clicks `"Create"`;  
Then a new issue should be created, status should be `"Open"`, the issue should have a unique ID, and the current user should be its author.

**Сценарій для АС-02:**  
Given the user is creating a new issue;  
When the user leaves the title empty and clicks `"Create"`;  
Then the issue should not be created and the system should display `"Title is required"`.

**Source:** §5 / US-01 / сценарії для АС-01 та АС-02; окремих нових numeric IDs для цих сценаріїв у Pack не створюється.

---

## US-02 — View Issues

**Actor:** Team Member

**Мета:** переглядати зареєстровані заявки та їх поточні деталі.

**Source:** §5 / US-02.

### Acceptance Criteria

**AC-02.1** Авторизований член команди може переглянути список зареєстрованих заявок.  
**AC-02.2** Для кожної заявки у списку відображаються щонайменше: `ID`; `Title`; `Status`; `Priority`; `Assignee`, якщо він призначений.  
**AC-02.3** Користувач може вибрати заявку зі списку та відкрити сторінку її деталей.  
**AC-02.4** На сторінці відображаються: `ID`, `Title`, `Description`, `Status`, `Priority`, `Assignee` (якщо призначений), `Author`, `Created at`.  
**AC-02.5** Якщо зареєстрованих заявок немає, система відображає порожній список або повідомлення про відсутність.

**Source:** §5 / US-02 / AC-02.1–AC-02.5.

---

## US-03 — Assign Issue to the responsible team member

**Actor:** Manager (Team Lead)

**Мета:** призначити заявку члену команди, щоб було зрозуміло, хто відповідає за її виконання.

**Source:** §5 / US-03.

### Acceptance Criteria

У джерелі наведено чотири **ненумеровані** критерії:

1. можна вибрати одного `assignee`;  
2. `assignee` повинен бути членом команди;  
3. призначати заявку може лише Manager (Team Lead);  
4. поточний `assignee` відображається у заявці.

**Source:** §5 / US-03 / Acceptance Criteria 03. Ідентифікатори AC не вигадуються.

---

## US-04 — Set Issue Priority

**Actor:** Manager (Team Lead)

**Мета:** встановлювати пріоритет заявки, щоб команда могла визначити порядок роботи.

**Source:** §5 / US-04.

### Acceptance Criteria

**АС-04.1** Допустимі значення встановленого Priority: `Low`, `Normal`, `High`, `Critical`.  
**АС-04.2** Встановлювати та змінювати priority може лише Manager (Team Lead).

**Source:** §5 / US-04 / АС-04.1–АС-04.2.

---

## US-05 — Track Issue Progress

**Actor:** Team Member

**Мета:** бачити поточний status, щоб розуміти поточний прогрес роботи над перекладом.

**Source:** §5 / US-05.

### Acceptance Criteria

**АС-05.1** Поточний `status` відображається для кожної заявки.  
**AC-05.2** `Status` відображається на сторінці заявки.  
**AC-05.3** `Status` відображається у списку.

**Source:** §5 / US-05 / АС-05.1, AC-05.2, AC-05.3.

---

## US-06 — Add Comment

**Actor:** Team Member

**Мета:** додавати коментарі до заявки для обговорення робочих моментів.

**Source:** §5 / US-06.

### Acceptance Criteria

**AC-06.1** `Comment text` cannot be empty.  
**AC-06.2** Added comment is stored and shown for the issue.  
**AC-06.3** Comment author is stored and displayed.  
**AC-06.4** Creation date/time is stored and displayed.  
**AC-06.5** Failed save does not create a comment and produces an error.

**Source:** §5 / US-06 / AC-06.1–AC-06.5.

---

## US-07 — Update Issue Status

**Actor:** член команди, відповідальний за заявку.

**Мета:** змінювати статус заявки, щоб команда бачила поточний стан роботи над перекладом.

**Source:** §5 / US-07.

### Acceptance Criteria

**АС-07.1** Нова заявка створюється зі статусом `Open`.  
**АС-07.2** Допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed`.  
**АС-07.3** Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді.  
**АС-07.4** Користувач, який не є виконавцем заявки, не може змінити її статус.  
**АС-07.5** Система не дозволяє встановити значення статусу, якого немає у визначеному переліку.

**Source:** §5 / US-07 / АС-07.1–АС-07.5.

---

## US-08 — View Important Open Issues

**Actor:** Team Member

**Мета:** швидко бачити важливі відкриті заявки, щоб зосередитися на перекладах, які потребують першочергової уваги.

**Source:** §5 / US-08.

### Acceptance Criteria

**AC-08.1** Заявка з `priority = High` і статусом, відмінним від `Closed`, відображається у списку Important Issues.  
**AC-08.2** Заявка з `priority = Critical` і статусом, відмінним від `Closed`, відображається у списку Important Issues.  
**AC-08.3** Заявки з `priority = Low` або `Normal` не відображаються у списку Important Issues.  
**AC-08.4** Заявки зі `status = Closed` не відображаються незалежно від priority.

**Source:** §5 / US-08 / AC-08.1–AC-08.4.

---

# 4. UI-Relevant Business Rules

## 4.1 Rights and access

**BR-01 — Authorized users**  
Системою можуть користуватися лише автентифіковані члени команди. У першій версії є три заздалегідь визначені користувачі: два Translator та один Manager.

**Source:** §3 / BR-01.

**BR-02 — Issue creation**  
Будь-який автентифікований член команди може створити запит на переклад.

**Source:** §3 / BR-02.

**BR-06 — Priority management**  
Встановлювати та змінювати priority заявки може тільки Manager (Team Lead).

**Source:** §3 / BR-06.

**BR-08 — Issue status management**  
Член команди, відповідальний за виконання заявки, повинен мати можливість змінювати її status.

**Source:** §3 / BR-08.

**US-03** додатково встановлює, що призначати заявку може лише Manager, а assignee повинен бути членом команди.

**Source:** §5 / US-03 / Acceptance Criteria 03.

### Заборонені дії щодо прав

- неавторизований користувач не має права користуватися системою — §3 / BR-01;
- Translator не має права встановлювати або змінювати priority — §3 / BR-06, §5 / US-04 / АС-04.2; - не-Manager не має права призначати заявку — §5 / US-03 / Acceptance Criteria 03;
- користувач, який не є виконавцем заявки, не може змінити її status — §5 / US-07 / AC-07.4.

---

## 4.2 Initial values

**BR-03 — Initial issue status**  
Під час створення нової заявки система автоматично встановлює `status = Open`.

**Source:** §3 / BR-03; також §5 / US-01 / AC-01.4 та US-07 / АС-07.1. 
**BR-04 — Initial issue priority**  
Під час створення нової заявки її `priority` не визначений: `priority = Not Set`.

**Source:** §3 / BR-04; §5 / US-01 / AC-01.5. 
**BR-05 — Allowed priority values**  
Після встановлення priority воно повинно мати одне зі значень `Low`, `Normal`, `High`, `Critical`; до встановлення priority допускається стан «Не встановлено» (пусте значення).

**Source:** §3 / BR-05.

**Важливо для UI:** `Not Set` не слід прирівнювати до жодного з установлених рівнів `Low / Normal / High / Critical`. З джерела випливає лише те, що це окремий стан «не встановлено»; точний спосіб його візуального подання не заданий.

**Type:** DERIVED  
**Source:** §3 / BR-04 + BR-05; §5 / US-01 / AC-01.5. 
---

## 4.3 Status values and transitions

**BR-07 — Allowed status values**  
Статус заявки може мати лише: `Open`, `In Progress`, `Resolved`, `Closed`.

**Source:** §3 / BR-07.

**BR-08.1 — Status transitions**

Дозволені переходи:

- `Open → In Progress`
- `In Progress → Resolved`
- `Resolved → Closed`
- `Resolved → In Progress` — для повторного відкриття роботи
- `Open → Closed` — для випадку скасування клієнтом до початку обробки

**Інші переходи не дозволені.**

**Source:** §3 / BR-08.1.

### Заборонені переходи

Усі переходи, які не входять до переліку BR-08.1, є недозволеними.

Зокрема, зі специфікації прямо випливає, що `Closed → ...` не визначений серед дозволених переходів і тому підпадає під правило «Інші переходи не дозволені».

**Type:** DERIVED  
**Обґрунтування:** §3 / BR-08.1 прямо визначає дозволений перелік та забороняє інші переходи.

**Не прирівнювати `Resolved` до `Closed`.**  
`Resolved` означає, що заявка вирішена, але ще не закрита.

**Source:** §3 / BR-09.1.

---

## 4.4 Important Issues

**BR-09 — Important issue**  
Заявка вважається важливою, якщо її priority має значення `High` або `Critical`.

**Source:** §3 / BR-09.

**BR-09.1**  
Заявка зі статусом `Resolved` вважається вирішеною, але ще не закритою. До переходу в `Closed` вона залишається активною та може відображатися серед Important Issues відповідно до її priority.

**Source:** §3 / BR-09.1.

Для конкретної поведінки Important Issues у Release MVP застосовуються AC-08.1–AC-08.4:

- `High` + status ≠ `Closed` → показувати;
- `Critical` + status ≠ `Closed` → показувати;
- `Low` / `Normal` → не показувати;
- `Closed` → не показувати незалежно від priority.

**Source:** §5 / US-08 / AC-08.1–AC-08.4.

**Type:** DERIVED  
**Висновок:** `Resolved + High/Critical` може залишатися в Important Issues, тому `Resolved` не є еквівалентом `Closed`.

**Source:** §3 / BR-09.1 + §5 / US-08 / AC-08.1–AC-08.4. 
---

## 4.5 Comments

Для коментаря:

- текст не може бути порожнім;
- доданий comment зберігається та відображається для відповідної заявки;
- зберігається та відображається автор comment;
- зберігається та відображається дата/час створення;
- при невдалому збереженні comment **не створюється** та система генерує помилку.

**Source:** §5 / US-06 / AC-06.1–AC-06.5.

**Критична негативна умова:** показ помилки збереження не повинен трактуватися як успішне створення comment.

**Type:** DERIVED  
**Source:** §5 / US-06 / AC-06.2 + AC-06.5.

Специфікація не задає права на редагування або видалення уже створених comment; такі права не додаються до Pack.

---

## 4.6 Issue deletion and change history

**BR-10 — Issue deletion:** заявки не видаляються із системи.

**Source:** §3 / BR-10.

**BR-11 — Change history:** окрема користувацька історія змін у першій версії не передбачається.

**Source:** §3 / BR-11.

**UI implication:** не слід вимагати від макета функції видалення заявки або окремого user-facing change history.

---

# 5. Relevant NFR

| ID | Requirement | UX/UI relevance | Чи перевіряється макетом |
|---|---|---|---|
| NFR-01 | Response time < 1 s | визначає очікувану швидкодію взаємодій | **Ні**, статичний макет цього не доводить |
| NFR-02 | Authentication required | визначає, що доступ має бути обмежений автентифікованими користувачами | **Не повністю**; наявність/структура auth UI сама по собі не доводить реалізацію автентифікації |
| NFR-03 | HTTPS only | вимога до каналу доступу | **Ні**, не перевіряється макетом |
| NFR-04 | Backup of all data | вимога до збереження/відновлення даних | **Ні**, не перевіряється макетом |

**Source:** §8 / Updated NFRs.

Додатково в §6 попередня версія NFR-04 містить формулювання **“Backup of all data + attachments”**, тоді як у §8 фінальна версія містить **“Backup of all data”**. Ця різниця зафіксована окремо в Q-ID і не вважається автоматично скасованою лише через те, що §8 іде пізніше.

**Source:** §6 / NFR-04 та §8 / Updated NFR-04. 
### Вимоги, які не перевіряються макетом

- фактичний response time `< 1 s`;
- фактична реалізація authentication;
- HTTPS;
- backup;
- фактичне збереження даних;
- фактична коректність заборонених переходів;
- фактична неможливість створити comment після failed save.

Макет може показати передбачену взаємодію та стани, але не доводить runtime-поведінку.

**Type:** DERIVED  
**Source:** §6 / NFR-01–NFR-04; §5 / US-06 / AC-06.5; §5 / US-07 / AC-07.3–AC-07.5. 
---

# 6. Q-ID — Conflicts, Gaps and Human Decisions

## Q-01 — Description: text or link

**Type:** UNRESOLVED

**Issue:** Початковий Customer Request визначає description як **“текст або посилання”**, тоді як US-01 / AC-01.1 лише визначає поле `Description`, а AC-01.3 — що воно обов’язкове. Формат/правило для посилання в деталізованій US не визначені.

**Source:** §0 / Customer Request; §5 / US-01 / AC-01.1, AC-01.3. 
**Потребує рішення людини:** чи має `Description` приймати довільний текст, посилання, або обидва варіанти без окремого UX-правила.

---

## Q-02 — Meaning of “Open” in US-08

**Type:** UNRESOLVED

**Issue:** Назва US-08 — **“View Important Open Issues”**, але AC-08.1–AC-08.2 визначають включення всіх заявок із `High/Critical` та status, відмінним від `Closed`, тобто також `Resolved`. BR-09.1 прямо вказує, що `Resolved` залишається активною до `Closed` і може бути Important Issue.

**Source:** §5 / US-08; §5 / AC-08.1–AC-08.4; §3 / BR-09.1. 
**Потрібне рішення:** чи назва “Open Issues” є лише бізнес-назвою списку, який фактично означає “не Closed”, чи термін “Open” повинен відповідати буквально status `Open`.

У Pack не скасовується жодна з цих частин; суперечність залишається зафіксованою.

---

## Q-03 — Open → Closed через cancellation

**Type:** UNRESOLVED

**Issue:** BR-08.1 дозволяє `Open → Closed` у випадку скасування клієнтом до початку обробки. Водночас BR-08 встановлює, що status може змінювати член команди, відповідальний за виконання заявки; окремого правила про право Manager закривати заявку при cancellation немає. При цьому зовнішні клієнти не є користувачами системи.

**Source:** §3 / BR-08, BR-08.1; §1 / КС-04; §5 / US-07 / AC-07.4. 
**Потрібне рішення:** хто саме має право виконати `Open → Closed` при cancellation, якщо assignee ще не працює із заявкою або не призначений.

---

## Q-04 — NFR-04 і attachments

**Type:** UNRESOLVED

**Issue:** §6 містить `Backup of all data + attachments`, але §8 у Updated NFRs містить `Backup of all data`; водночас US-09 з attachments має статус Deferred.

**Source:** §6 / NFR-04; §5 / US-09; §8 / Updated NFR-04. 
**Потрібне рішення:** чи трактувати Updated NFR-04 як зміну області дії backup для поточного релізу, і окремо коли/як NFR щодо attachments набуває чинності.

---

## Q-05 — Representation of Not Set

**Type:** UNRESOLVED

**Issue:** BR-04 визначає `priority = Not Set`; BR-05 одночасно зазначає, що до встановлення допускається стан «Не встановлено» як пусте значення. Джерело не визначає, чи `Not Set` має бути явним UI-значенням, чи лише семантикою відсутності встановленого priority.

**Source:** §3 / BR-04, BR-05.

**Потрібне рішення:** узгодити точне візуальне подання `Not Set`.

При цьому **не допускається** трактувати `Not Set` як `Low`, `Normal`, `High` або `Critical`.

---

## Q-06 — Initial assignee

**Type:** UNRESOLVED

**Issue:** Для нової заявки явно задані initial `status = Open` та `priority = Not Set`, а `assignee` у US-02 відображається “якщо він призначений”. Початкове значення assignee окремо не визначене.

**Source:** §3 / BR-03, BR-04; §5 / US-01 / AC-01.4–AC-01.5; §5 / US-02 / AC-02.2, AC-02.4. 
**Потрібне рішення:** чи нова заявка початково має бути без assignee, чи існує інше погоджене initial value.

---

## Q-07 — Empty list UI

**Type:** UNRESOLVED

**Issue:** AC-02.5 дозволяє два варіанти: порожній список **або** повідомлення про відсутність заявок.

**Source:** §5 / US-02 / AC-02.5.

**Потрібне рішення:** який із двох варіантів є очікуваним для фінального UI.

---

## Q-08 — Status transition control semantics

**Type:** UNRESOLVED

**Issue:** Специфікація визначає дозволені переходи, але не визначає, як поводитися в UI з недозволеними переходами: не показувати їх чи показувати як недоступні. Обидва варіанти є UI-механізмами, але джерело не задає конкретної форми.

**Source:** §3 / BR-08.1; §5 / US-07 / AC-07.4–AC-07.5. 
**Потрібне рішення:** погодити спосіб відображення недозволених переходів.

---

## Q-09 — Authentication method

**Type:** UNRESOLVED

**Issue:** Authentication є обов’язковою, але спосіб її реалізації прямо визначений як ризик, що потребує уточнення.

**Source:** §1 / Open Questions: authentication required; §7 / R-01. 
**Потрібне рішення:** конкретний спосіб автентифікації.

**Обмеження Pack:** жоден спосіб authentication не додається як погоджений.

---

## Q-10 — Date/time presentation

**Type:** UNRESOLVED

**Issue:** AC-01.4 вимагає `created_at`, а AC-06.4 — creation date/time comment, але формат, timezone та правила локалізації не визначені.

**Source:** §5 / US-01 / AC-01.4; §5 / US-06 / AC-06.4. 
**Потрібне рішення:** формат відображення дат/часу та timezone, якщо це потрібно для остаточного UI.

---

## Q-11 — PROPOSED: explicit visual distinction of Not Set

**Type:** PROPOSED

**Твердження:** у UI доцільно візуально показувати `Not Set` як окремий стан, а не маскувати його під один із чотирьох priority levels.

**Source:** §3 / BR-04, BR-05.

**Статус:** це **UX-пропозиція**, а не нова обов’язкова вимога. Вона не розширює модель даних і не додає нового priority.

---

# 7. Important Source-Level Contradictions / Scope Evolution

### C-01 — Candidate MVP vs final Release Scope

У §4 Candidate MVP включає US-01..US-08. У фінальному §8 Release Scope також встановлює US-01..US-08. Отже, у цьому випадку Candidate MVP і фінальний MVP збігаються, але визначальним для Pack є саме §8.

**Source:** §4 / Candidate MVP; §8 / Release MVP. 
**Type:** DERIVED

### C-02 — SR-09 vs US-09 / final scope

SR-09 описує attachments як stakeholder requirement; пізніше деталізована US-09 позначена DEFERRED, а фінальний §8 переносить US-09 за межі Release MVP.

Це є еволюцією scope, але Pack **не вилучає саму SR-09 з трасування**: її реалізація у першому релізі представлена деталізованою US-09, яка має статус Deferred.

**Source:** §3 / SR-09; §4 / SR-09 priority; §5 / US-09; §8 / Deferred US-09. 
**Type:** DERIVED

### C-03 — US-08 “Open” vs фактичний критерій “status ≠ Closed”

Назва US-08 використовує “Open”, але AC-08.1–AC-08.2 та BR-09.1 охоплюють `Resolved`.

**Source:** §3 / BR-09.1; §5 / US-08 / AC-08.1–AC-08.4. 
**Type:** UNRESOLVED — див. Q-02.

### C-04 — NFR-04: attachments

§6 містить backup для `all data + attachments`, §8 — лише `all data`.

**Source:** §6 / NFR-04; §8 / Updated NFR-04. 
**Type:** UNRESOLVED — див. Q-04.

---

# 8. Design-Relevant Derived Conclusions

## D-01 — Issue detail must support all required issue information

Оскільки US-02 / AC-02.4 прямо вимагає відображати `ID`, `Title`, `Description`, `Status`, `Priority`, `Assignee` (якщо призначений), `Author`, `Created at`, ці дані мають бути доступними в UI деталей заявки.

**Type:** DERIVED  
**Source:** §5 / US-02 / AC-02.3–AC-02.4.

## D-02 — Issue list must show status and priority and conditional assignee

Список має відображати щонайменше `ID`, `Title`, `Status`, `Priority`, а `Assignee` — якщо призначений.

**Type:** SOURCE  
**Source:** §5 / US-02 / AC-02.2.

## D-03 — Priority is permission-controlled

Для ролі Translator UI не повинен передбачати активну можливість установлення/зміни priority; право належить Manager.

**Type:** DERIVED  
**Source:** §3 / BR-06; §5 / US-04 / АС-04.2. 
## D-04 — Status is assignee-controlled

Користувач, який не є виконавцем заявки, не може змінити status.

**Type:** SOURCE  
**Source:** §5 / US-07 / AC-07.4.

## D-05 — Important Issues excludes Closed regardless of priority

`Closed` завжди виключається з Important Issues, навіть якщо priority = `High` або `Critical`.

**Type:** SOURCE  
**Source:** §5 / US-08 / AC-08.4.

## D-06 — Resolved is not Closed

`Resolved` залишається активним до переходу в `Closed` і за `High/Critical` може бути в Important Issues.

**Type:** DERIVED  
**Source:** §3 / BR-09.1 + §5 / US-08 / AC-08.1–AC-08.2. 
## D-07 — Delete action must not be part of the issue workflow

Окремої дії видалення заявки в UI не повинно бути як реалізації BR-10, оскільки заявки не видаляються.

**Type:** DERIVED  
**Source:** §3 / BR-10.

## D-08 — User-facing change history is not an MVP requirement

Окремий user-facing change history не входить у перший реліз.

**Type:** SOURCE  
**Source:** §3 / BR-11.

---

# 9. Non-Requirements / Do Not Infer

До Pack **не додаються як погоджені вимоги**:

- конкретний спосіб authentication;
- нові поля заявки, крім прямо згаданих у AC;
- нові ролі;
- зовнішні клієнти як користувачі;
- mobile application;
- attachments у Release MVP;
- AI categorization у Release MVP;
- automatic priority recommendation у Release MVP;
- edit/delete для comments;
- delete issue;
- окрема user-facing change history;
- конкретний механізм збереження даних;
- конкретний storage для attachments;
- будь-які нові workflow transition rules;
- конкретні назви чи кількість екранів;
- конкретна навігаційна структура.

**Source:** відповідні обмеження, BR, US, NFR, Release Scope та відсутність таких вимог у специфікації. 
---

# 10. Source Check

## 10.1 Included US → all AC → Pack location

| Included US | Усі критерії/сценарії з джерела | Місце в Pack |
|---|---|---|
| **US-01 Create Issue** | AC-01.1, AC-01.2, AC-01.3, AC-01.4, AC-01.5; сценарій для АС-01; сценарій для АС-02 | §3 US-01; §4 Initial values; §6 Q-01, Q-05, Q-06 |
| **US-02 View Issues** | AC-02.1, AC-02.2, AC-02.3, AC-02.4, AC-02.5 | §3 US-02; §6 Q-06, Q-07; §8 D-01, D-02 |
| **US-03 Assign Issue...** | 4 ненумеровані Acceptance Criteria: one assignee; assignee is team member; only Manager; current assignee displayed | §3 US-03; §4 Rights; §6 Q-03, Q-06 |
| **US-04 Set Issue Priority** | АС-04.1, АС-04.2 | §3 US-04; §4 Priority; §8 D-03 |
| **US-05 Track Issue Progress** | АС-05.1, AC-05.2, AC-05.3 | §3 US-05 |
| **US-06 Add Comment** | AC-06.1, AC-06.2, AC-06.3, AC-06.4, AC-06.5 | §3 US-06; §4 Comments; §5 NFR/UI-test boundary |
| **US-07 Update Issue Status** | АС-07.1, АС-07.2, АС-07.3, АС-07.4, АС-07.5 | §3 US-07; §4 Rights/Transitions; §6 Q-03/Q-08; §8 D-04 |
| **US-08 View Important Open Issues** | AC-08.1, AC-08.2, AC-08.3, AC-08.4 | §3 US-08; §4 Important Issues; §6 Q-02; §8 D-05, D-06 |

**Source:** §5 / US-01–US-08 та §3 / BR. 
---

## 10.2 Excluded requirements and reason

| Requirement | Status | Причина виключення з Release MVP |
|---|---|---|
| **US-09 Attach files to an issue** | Deferred | Фінальний §8: Deferred; висока складність і додаткові технічні залежності |
| **SR-10 Automatic issue categorization** | Future | Фінальний §8: Future |
| **SR-11 Automatic Priority recommendation** | Future | Фінальний §8: Future |
| **SR-09 / attachments capability** | Deferred through US-09 | Деталізована US-09 перенесена за межі першого релізу |
| Customer support portal | Explicitly Out of Scope | §2 Out of Scope |
| Billing | Explicitly Out of Scope | §2 Out of Scope |
| CI/CD | Explicitly Out of Scope | §2 Out of Scope |
| Time tracking | Explicitly Out of Scope | §2 Out of Scope |
| Full project management | Explicitly Out of Scope | §2 Out of Scope |
| Mobile application | Explicitly Out of Scope | §2 Out of Scope / КС-03, КС-07 |
| Зовнішні tracker/CRM integrations | Not planned for v1 | КС-05 |
| Зовнішні клієнти як користувачі | Out of first-release user boundary | КС-04 |

**Source:** §2 / Initial Product Scope & Out of Scope; §3 / SR-09–SR-11; §5 / US-09; §8 / Release Scope. 
---

## 10.3 Unresolved questions

1. **Q-01:** формат/поведінка `Description`: текст, посилання чи обидва.
2. **Q-02:** значення слова “Open” у назві US-08 щодо включення `Resolved`.
3. **Q-03:** хто саме може виконати `Open → Closed` при client cancellation.
4. **Q-04:** актуальна область дії NFR-04 щодо backup attachments.
5. **Q-05:** точне UI-подання `Not Set`.
6. **Q-06:** initial value для `Assignee`.
7. **Q-07:** який варіант empty state обирається для порожнього списку.
8. **Q-08:** як у UI представляти недозволені status transitions.
9. **Q-09:** конкретний спосіб authentication.
10. **Q-10:** формат і timezone для date/time.

**Source:** відповідні пункти §1 Open Questions, §3 BR, §5 US/AC, §6 NFR та §7 Risks. 
---

# 11. Final Pack Boundary

Для UX/UI Design наступним етапом може використовуватися лише погоджений фактологічний baseline цього Pack:

**Release MVP:** US-01–US-08.  
**Deferred:** US-09.  
**Future:** SR-10, SR-11.  
**Explicitly Out of Scope:** Customer support portal, Billing, CI/CD, Time tracking, Full project management, Mobile application та інші прямо виключені напрями.

Суперечності та прогалини, перелічені в Q-01–Q-10, **не вважаються вирішеними**. Жодна PROPOSED-позиція не є обов’язковою вимогою. Pack **не оголошується погодженим**.