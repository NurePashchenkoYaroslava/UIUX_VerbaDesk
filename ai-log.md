# AI Interaction Log

**Проєкт:** Mini Translation Tracker (Бюро перекладів)
**Автори:** Нікітіна Олександра Володимирівна, Пащенко Ярослава Олегівна
**Дата початку:** 01.10.2026

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 01.10.2026 / Нікітіна О.В., Пащенко Я.О. / G1 (Створення Input Pack) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | `Translation_Spec.docx` (повний адаптований текст специфікації) |
| **Prompt / response** | **Запит:** "Ти — аналітик вимог. Створи UX/UI Design Input Pack лише з прикріпленої актуальної специфікації Mini Translation Tracker..."<br>**Відповідь:** Згенеровано файл `01_Input_Pack_v0.1.md` із виділеними питаннями (Q-01 - Q-10). |
| **Decision / evidence** | Прийнято. Перевірено повноту: включено всі 8 US та 33 AC, джерела збережено. Згенерований текст скопійовано у файл `01_Input_Pack_v0.1.md` без змін. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 01.10.2026 / Нікітіна О.В., Пащенко Я.О. / Узгодження Decision Log та фіналізація Input Pack |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | Контекст попередньої розмови + ручні рішення D-01 до D-14. |
| **Prompt / response** | **Запит:** "Перед продовженням зафіксуй лише рішення зі статусом APPROVED у наведеному Decision Log. Поверни компактний Input Pack v1.0..."<br>**Відповідь:** Згенеровано фінальний `VerbaDesk — UX-UI Input Pack v1.0.md`. |
| **Decision / evidence** | Прийнято. Перевірено, що всі питання Q-01–Q-10 успішно закриті нашими рішеннями D-01–D-14. Файл збережено як `Input_Pack_v1.0.md`. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 01.10.2026 / Нікітіна О.В., Пащенко Я.О. / G2 (Послідовність екранів) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | `Input_Pack_v1.0.md` + `Decision_Log.md` |
| **Prompt / response** | **Запит:** "Ти - UX architect. Використовуй останній погоджений Input Pack і Decision Log... Побудуй мінімальну систему екранів..."<br>**Відповідь:** Згенеровано перелік екранів, станів та таблицю переходів. |
| **Decision / evidence** | Прийнято на перевірку. Результат скопійовано у файл `02_Screen_Map_v0.1.md`. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 02.10.2026 / Нікітіна О.В., Пащенко Я.О. / G3 (Контракти екранів) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | `02_Screen_Map_v0.1.md` + попередні контексти. |
| **Prompt / response** | **Запит:** "Підготуй Developer-Verifiable Screen Contracts за погодженим планом..."<br>**Відповідь:** Згенеровано `03_Screen_Contracts.md` із пропозиціями D-15...D-21. |
| **Decision / evidence** | Прийнято. Збережено у файл `03_Screen_Contracts.md`. Вирішено погодити запропоновані D-15...D-21 перед малюванням макета. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 02.10.2026 / Нікітіна О.В., Пащенко Я.О. / G4 (Перевірка плану) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | `03_Screen_Contracts.md` |
| **Prompt / response** | **Запит:** "Перевір план як критичний рецензент..."<br>**Відповідь:** План отримав статус NOT READY через невраховані рішення щодо навігації після створення (DEF-01) та текстів помилок, які ми ще не встигли погодити. |
| **Decision / evidence** | Прийнято до виправлення. Усі знайдені проблеми DEF-01 – DEF-07 закриваються нашими погодженими рішеннями (D-15...D-24). Відправляємо ШІ запит на оновлення контракту. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 05.10.2026 / Нікітіна О.В., Пащенко Я.О. / G4 (Завершення перевірки та отримання READY) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | Screen Contracts v1.0 + Critical Review + D-15-D-22 |
| **Prompt / response** | **Запит:** "Ми офіційно затверджуємо наступні рішення (APPROVED D-Decisions), які закривають знайдені тобою дефекти DEF-01 – DEF-07... зміни фінальний статус перевірки на PLAN READY."<br>**Відповідь:** ШІ підтвердив закриття дефектів DEF-01-07 та змінив статус на PLAN READY. |
| **Decision / evidence** | План екранів та контракти затверджено. Готовність до створення Figma Brief підтверджено. |
| **Limit / fallback** | Ліміт оновлено, запит виконано успішно. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 05.10.2026 / Нікітіна О.В., Пащенко Я.О. / G5 (Генерація Figma Brief) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | Затверджений Screen Contracts v1.0 |
| **Prompt / response** | **Запит:** "Скомпілюй один self-contained англомовний Figma Design implementation brief з погоджених Screen Contracts..."<br>**Відповідь:** Згенеровано англомовний текст брифа, що містить purpose and scope, constraints, canonical screens, controls, status transitions та acceptance checklist. |
| **Decision / evidence** | Згенерований текст збережено у файл `05_Figma_Brief_v1.0.md`. Макет готовий до передачі у Figma Agent. |
| **Limit / fallback** | Обмежень не зафіксовано. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 05.10.2026 / Нікітіна О.В., Пащенко Я.О. / F1 (Основна генерація макета у Figma) |
| **Tool / plan / mode** | Figma Agent / Design file |
| **Context / version** | Файл `05_Figma_Brief_v1.0.md` завантажено як контекст. |
| **Prompt / response** | **Запит:** "Use the attached 05_Figma_Brief_v1.0.md as the approved design contract for VerbaDesk..." (базовий F1-Launcher).<br>**Відповідь:** Figma Agent згенерував low-fidelity екрани (Wireframes_v1) та надав текстовий звіт про створені компоненти. |
| **Decision / evidence** | Збережено бекап файлу (`BEFORE_REVIEW.fig`). Проведено візуальний огляд (Review) людиною. Відхилень не виявлено. |
| **Limit / fallback** | Генерація пройшла успішно. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 05.10.2026 / Нікітіна О.В., Пащенко Я.О. / P1 (Прототипування та ручні правки) |
| **Tool / plan / mode** | Figma (ручна робота) |
| **Context / version** | Макет AFTER_REVIEW, згенерований Figma Agent. |
| **Prompt / response** | **Дія:** Ручне налаштування інтерактивних переходів у вкладці Prototype. |
| **Decision / evidence** | Створено зв'язки згідно з Transition matrix: `S-02` -> `M-01` (overlay); `M-01` (Create/Close) -> `S-03` / `S-02`; `S-02` -> `S-03` -> `Back`. Прототип успішно протестовано на базовому сценарії. |
| **Limit / fallback** | Етап виконано повністю вручну (без AI), оскільки створення прототипів виходить за межі можливостей Figma Agent. |

| Поле | Деталі взаємодії |
| :--- | :--- |
| **Date / operator / step** | 05.10.2026 / Нікітіна О.В., Пащенко Я.О. / G6 (Фінальний аудит доказів - Review Report) |
| **Tool / plan / mode** | ChatGPT / text-only |
| **Context / version** | PDF-файл з усіма екранами + погоджений Screen Contracts v1.0. |
| **Prompt / response** | **Запит:** "Зроби фінальний аудит макета згідно з вимогами методички (Крок G6)."<br>**Відповідь:** Згенеровано `Review Report`. Підтверджено наявність усіх обов'язкових полів (AC-01.1-3, AC-02.2), дотримання логіки Important Issues (AC-08.1-4) та прав доступу (disabled controls). Дефектів не виявлено. |
| **Decision / evidence** | Звіт про перевірку (Review Report) збережено у файл `06_Review_Report.md`. Проєкт визнано повністю готовим до здачі. |
| **Limit / fallback** | Обмежень не зафіксовано. |