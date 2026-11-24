# Оформление ОТ-АВГУСТ

## Источники
- Linear, UI из открытой страницы Refero: https://refero.design/pages/a952c8f5-b355-4505-97e7-3a9c8254a949
- OpenAI, UI из открытой страницы Refero: https://refero.design/pages/41c7f753-5d4a-47a1-982a-769ad7396ca8
- Linear Changelog, описание стиля Refero: https://styles.refero.design/style/a5cc9b0f-d274-458a-b990-d18482b70838

Публичные изображения двух интерфейсов просмотрены. Доступ через Refero MCP недоступен: NO_SUBSCRIPTION. Прямой сайт использован через открытые индексируемые страницы и публичные изображения.

## Зафиксированные решения
- Из Linear UI: боковая навигация и единое поле документа; без отдельных карточек на фоне.
- Из OpenAI UI: ровные колонки подписей и контента, тонкие разделители списков.
- Из Linear Changelog: нейтральные тёмные поверхности, компактная шкала отступов 4/8/12/16/24/32/48, умеренные начертания и минимальная декоративность.
- По просьбе пользователя: SF через системный стек Apple, постоянная тёмная тема, синий только в действиях и выбранных состояниях.
- По последнему уточнению: все настройки открыты сразу. Количество 20/50/Все и время — сегментированные кнопки; темы — полный список с радиокнопками. Скрытых меню настроек нет.
- Исходные изображения используются только для иллюстраций и сверки. Вопросы и ответы отображаются текстом.

## Проверка
Safari macOS: открытые настройки, переключение времени, изменение раздела, запуск всех 12 вопросов Электробезопасности.
Responsive Design Mode 390px: нет переполнения кнопок, длинные названия разделов переносятся; действия доступны с телефона.

## Движение
Правила из Refero Design / motion.md: отклик 120 мс, смена вопроса 200 мс с перемещением 4px, диалог 240 мс. Все анимации отключаются при prefers-reduced-motion.

## Изменения по прямому запросу пользователя
Верхняя навигация вместо боковой; без скруглений. Вопрос занимает доступную высоту, нижние кнопки закреплены. Зелёный/красный — только результат проверки, синий контур — текущий номер. Пропущенные без заливки. На телефоне навигатор остаётся доступен сверху.

## Финальная полировка
Переходы страниц и истории 200 мс, проверка ответа 180 мс, отметка 160 мс, кнопки 90–160 мс. При проверке ответа вопрос не появляется заново, анимируется только результат. prefers-reduced-motion отключает движение. Прерванные попытки не сохраняются и не отображаются в истории.

## Логотип по референсу пользователя
Светлая надпись avgust на тёмном фоне и три цветных знака; подпись ОТ-АВГУСТ. Палитра #40bfe0 / #54be36 / #ffb400 применяется в прогрессе. Логотип — кнопка перезагрузки с сохранением попытки. Индикатор исходного текста появляется до начала запроса.

Оригинальные контуры логотипа взяты из SVG https://avgust.com.by/upload/slam.options/c48/o2sy44nsklxhv8zc9439hu4k0rexfogf/logo-_1_.svg . Удалён отдельный путь нижней подписи, изменён цвет букв под тёмную тему, справа добавлен TEST. Форма букв и цветных знаков сохранена. Favicon — оригинальные PNG 32/48/192 с avgust.com.by. Досрочный выход даёт разбор без истории.


## Setup layout revision
Reference lock: retain the existing user-approved dark square interface and align the setup canvas with the quiz (1600px maximum, 28px side padding). Refero live research returned NO_SUBSCRIPTION; use the supplied screenshot and bundled motion/forms craft guidance.
- Layout: two open columns for participant/mode and question-count/time, separated by a single rule; topics and question library span the canvas. Mobile uses one column.
- State: keep the form DOM and focused control when choices change. Show timer minutes beside its selector; hide inactive fields from focus and validation. Exam displays its fixed duration and error limit.
- Motion: 160–200ms opacity and grid expansion only for dependent settings; buttons update in place. Reduced motion removes transitions.
- Tokens: preserve SF system font, dark canvas, blue selected controls, no radius, no shadows; existing brand gradient remains progress-only.


## Long-question viewport fix
The old question grid retained `align-items:start`, so a long question could size its grid item from content and push navigation below the viewport. The quiz now uses a bounded flex column and a stretching `minmax(0,1fr)` grid row. Only the question body scrolls; action buttons stay visible. Long stems receive compact typography; same-question answer renders preserve the scroll offset, new questions reset to the top. The scroll region is keyboard focusable.
Validation: Safari rendered all 625 available questions at desktop and 390×932, including wrong-answer feedback, last-option reachability and viewport bounds. 390×600 visually checked. All 27 figures and 3147 source image fragments verified and decoded by Safari. Live asset/source-bundle bytes checked against local files.

## Motion and loading polish — polish-22
Preserve the approved dark, square, SF system interface. Motion follows Refero's bundled Motion & Micro-interactions guide: 120ms feedback, 200ms state continuity, no blocking delays, reduced-motion support. Progress animates between actual previous/current values even when quiz DOM is replaced. Number navigation retains scroll and reveals the selected question. Expanded library and decoded source images fade in. Library/review figures use native lazy loading; live quiz figures remain eager. Source loading remains busy until every image decodes, with retry on decode failure and safe close during pending requests.
Safari's 390×600 sweep initially exposed document overflow on 32 long stems despite the visible footer remaining bounded. Layout/paint containment on the internal scroll region prevents descendant overflow from extending the document. All 625 questions then passed viewport, last-option and answer-scroll checks at 390×600. Desktop sweep also passed. Tests include progress animation/reduced-motion, retained navigation scroll, delayed image decode and image-error retry.

## Local history and resilience — resilience-27
Removed participant entry, participant filtering and visible names from setup, quiz, results and exports. History is a chronological list with date, mode, score, duration, review and confirmed deletion. Preserve previous completed results. Validate saved records after the bank loads: malformed entries, unknown IDs, invalid answer indexes and out-of-range navigation cannot crash startup. Final confirmation checks the deadline again and is tied to its original attempt. Confirmation callbacks run once. Network fetches abort after 15 seconds to offer retry. Export BOM and newline escapes corrected and verified.

## Simplified copy and first-load animation — simple-28
Removed service captions, footer disclaimers, participant UI and the repeat-mistakes action per user request. Existing history review opens all answers, highlighting the correct choice and the submitted wrong choice. First-load markup uses a text-free rotating original favicon, with an accessible loading label and reduced-motion static fallback. Critical animation styles are inline so the initial loader does not depend on external CSS.
