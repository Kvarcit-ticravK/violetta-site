# violetta-site — память проекта

Лендинг гештальт-терапевта Виолетты Надич. Живой сайт: https://violetta-nadych.com
(GitHub Pages, репо Kvarcit-ticravK/violetta-site, деплой = push в `main`).

## Архитектура (всё в одном `index.html`)
- **Двуязычность** UK(default)/RU. Словари `I18N.uk` / `I18N.ru` в `<script>`.
  Элементы с `data-i18n` (textContent) и `data-i18n-html` (innerHTML) переключает
  функция `setLang(lang)`. Она же обновляет `href` всех `.btn-wa` — предзаполненный
  текст WhatsApp на языке страницы (объект `waEnc` внутри `setLang`).
- **Контент** — в теге `<script id="content-overrides" type="application/json">`.
  JSON парсится в `OV`:
  - `OV.portrait` — фото, **base64 data-URL** (одно, лёгкое);
  - `OV.docs` — массив `{src, caption}`, `src` — **относительный путь** в `docs/`;
  - `OV.gallery` — планируемый слот галереи остальных сертификатов (ещё не наполнен);
  - `OV.video` — планируемый слот видео-визитки (секция скрыта, пока пусто).
  Потребители: `applyPortrait()`, `renderDocs()` и т.д. в конце файла.
- **Режим правок контента**: `?edit` в URL → правки в браузере → «Скачать» →
  файл заменяет `index.html` → push.

## Правила при правках
- **НЕ ломать `content-overrides`**: при редактировании HTML менять только
  содержимое этого тега; JSON держать валидным.
- **Тексты — в ОБОИХ словарях** (`I18N.uk` и `I18N.ru`), иначе рассинхрон языков.
- **Документы — файлами в `docs/`** (JPEG ≤1400px, ≤400KB), НЕ base64.
  Base64 только у портрета. Резерв (не на сайте) — `docs/reserve/`.
- **Проверять JS**: `new Function` на содержимом исполняемых `<script>` (Node).
- **После каждого push — проверка на живом домене** (https 200 + маркеры контента:
  «гештальт-терапевтка», `data:image`, нужные `docs/…`).
- Файл `CNAME` (violetta-nadych.com) не удалять — на нём висит домен и сертификат.

## Деплой
```
cd ~/violetta-site
git add -A && git commit -m "..." && git push origin main   # Pages соберёт за 1-2 мин
```
Автор коммитов: Kvarcit-ticravK / 999kvarcit999@gmail.com.

## Контакты на сайте (актуальные)
- WhatsApp: `wa.me/35799856038` (+357 99 856 038), с предзаполненным `?text=` по языку.
- Telegram: `t.me/+35799856038` (по номеру; TODO — заменить на username кипрского аккаунта).

## Доделки
- [ ] Галерея всех сертификатов (`OV.gallery`) + текст про мед-факультет + autocrop белых полей docs/ (PIL).
- [ ] Видео-визитка: наполнить `OV.video` (секция-слот уже готова, скрыта до контента).
- [ ] Telegram — username кипрского аккаунта вместо номера.
- [ ] Индексация: Google Search Console + Bing + Request Indexing.
- [ ] Вычитка украинского текста (?edit).
