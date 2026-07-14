# violetta-site — память проекта

Лендинг гештальт-терапевта Виолетты Надич. Живой сайт: https://violetta-nadych.com
(GitHub Pages, репо Kvarcit-ticravK/violetta-site, деплой = push в `main`).

## Статус (14.07.2026)
- **Видео-визитка живая** — два ролика (UK/RU), секция видна на обоих языках.
- **Сайт В ИНДЕКСЕ Google** — подтверждено 11.07, сниппет корректный.
- **Google Search Console** подключена (аккаунт vnadich@gmail.com), домен подтверждён
  TXT-записью, sitemap отправлен.
- HTTPS live (Let's Encrypt через GitHub, enforced, автопродление).

## Архитектура (всё в одном `index.html`)
- **Двуязычность** UK(default)/RU. Словари `I18N.uk`/`I18N.ru`. `setLang(lang)` переключает
  `data-i18n`/`data-i18n-html`, а также обновляет `href` кнопок `.btn-wa` (WhatsApp-prefill)
  и `.js-tg` (Telegram-prefill) — предзаполненный текст по языку.
- **Контент** — тег `<script id="content-overrides" type="application/json">` → объект `OV`:
  - `OV.portrait` — фото **base64 data-URL** (одно, лёгкое);
  - `OV.docs` — массив `{src, caption:{uk,ru}}`, `src` — путь в `docs/`;
  - `OV.gallery` — **12 сертификатов** `{src, caption:{uk,ru}}` (наполнено).
  Потребители: `applyPortrait/renderDocs/renderGallery`. Подписи docs/gallery
  билингвальны и перерисовываются в `setLang`.
- **Видео-визитка** — НЕ через `OV`, а отдельной константой `videoIds = {uk, ru}` (YouTube ID
  на каждый язык). `applyVideoLang(lang)` вызывается из `setLang`: вставляет YouTube-iframe,
  кэширует через `card.dataset.vid`; **пустой id → секция `#videoSec` скрыта целиком**.
  Наполнено: uk `nHMJZbkKv6k`, ru `j7FiLurHIro`. Карточка вертикальная (9:16, max 400px).
- **Фон**: слой `.bg-pattern` — бумажная текстура (десатур. fractalNoise, `position:absolute`
  → скроллится вместе с контентом, `opacity:.4`, `mix-blend-mode:multiply`) + `body::after`
  тонкое зерно поверх (4%). ⚠️ Прошли итерации олива→градиент→бумага; `olive-pattern.svg`
  удалён — сейчас именно бумажная текстура.
- **Режим правок контента**: `?edit` → правки в браузере → «Скачать» → push.

## Правила при правках
- **НЕ ломать `content-overrides`**: менять только содержимое тега; JSON держать валидным.
- **Тексты — в ОБОИХ словарях** (`I18N.uk` и `I18N.ru`).
- **Документы — файлами в `docs/`** (JPEG ≤1400px, галерея ≤1200px), НЕ base64. Base64 только
  у портрета. Резерв (не на сайте) — `docs/reserve/`.
- **Проверять JS**: `new Function` на содержимом исполняемых `<script>` (Node).
- **После каждого push — проверка на живом домене** (https 200 + маркеры контента).
- Файл `CNAME` не удалять — на нём висят домен и сертификат.

## Что добавлено 14.07
- Видео-визитка: механизм `OV.video`/`applyVideo` **заменён** на `videoIds`/`applyVideoLang`
  (показ ролика по языку), вписаны оба YouTube-ID, карточка переведена в вертикальный 9:16.

## Что добавлено 10–11.07
- Фоновая бумажная текстура (слой `.bg-pattern`).
- Favicon-комплект «фиалка»: `favicon.svg` + `favicon-32.png` + `favicon-192.png`
  + `apple-touch-icon.png` + legacy-тег `shortcut icon`.
- Фото-секции (формат `.about-photo`, клик → лайтбокс): перед «Про мене»
  (`docs/about-photo.jpg`) и перед «С чем ко мне приходять» (`docs/chat-photo.jpg`).
- Портрет hero открывается в лайтбоксе (кроме `?edit`).
- Instagram-ссылки (шапка + футер). Убран наклон/белые края портрета, оба фото одного
  размера (`min(320px,78vw)`), убрано «третій медичний факультет» из «Про мене».

## Деплой
```
cd ~/violetta-site
git add -A && git commit -m "..." && git push origin main   # Pages соберёт за 1-2 мин
```
Автор коммитов: Kvarcit-ticravK / 999kvarcit999@gmail.com.

## Контакты на сайте (финальные)
- Telegram: `t.me/violettanadych` (username, prefill по языку).
- WhatsApp: `wa.me/35799856038` (+357 99 856 038, prefill по языку).
- Instagram: `instagram.com/violettanadych` (**слитно!**), ссылка в шапке и футере.

## Незакрытое
- [ ] Вычитка текстов Виолеттой (`?edit`).
