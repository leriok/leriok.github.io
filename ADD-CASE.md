# Как добавить новый кейс

## Чеклист (кратко)

1. [ ] Подготовить и конвертировать ассеты в WebP
2. [ ] Скопировать `_case-template.html` → `новый-кейс.html`
3. [ ] Заполнить все плейсхолдеры в HTML
4. [ ] Заполнить переводы в JS (секция `translations`)
5. [ ] Вставить карточку в `about-me.html`
6. [ ] Добавить переводы карточки в `about-me.html`
7. [ ] Встроить новый кейс в цепочку `footer-next`
8. [ ] Обновить `CODEMAP.md`

---

## Шаг 1 — Ассеты

Все картинки конвертировать в WebP перед загрузкой:

```bash
cd cases/
# Один файл
python3 -c "from PIL import Image; img=Image.open('file.png'); img.save('file.webp', 'WEBP', quality=85)"

# Все PNG в папке разом
python3 -c "
from PIL import Image; from pathlib import Path
for p in Path('.').glob('*.png'):
    Image.open(p).save(p.with_suffix('.webp'), 'WEBP', quality=85)
    print('converted', p.name)
"
```

Видео MOV → MP4:
```bash
ffmpeg -i 'cases/video.mov' -vcodec h264 -acodec aac -crf 28 'cases/video.mp4'
```

Положить файлы в `cases/`. Именовать понятно, без кириллицы в расширении.

---

## Шаг 2 — HTML-файл кейса

Скопировать шаблон и переименовать:
```bash
cp _case-template.html новый-кейс.html
```

Найти все `TODO:` в файле (`Cmd+F → TODO:`) и заполнить:

| Плейсхолдер | Что вставить |
|---|---|
| `TODO: TITLE` | Название кейса, напр. `Новый Продукт` |
| `TODO: SLUG` | Имя файла без .html, напр. `new-product` |
| `TODO: DESC_RU` | Описание для meta/og (1–2 предложения, RU) |
| `TODO: DESC_EN` | То же на EN |
| `TODO: NEXT_CASE` | Имя следующего файла, напр. `coderabbit.html` |
| `TODO: OG_IMAGE` | Путь к hero-картинке в `cases/`, напр. `cases/Product hero.webp` |
| `TODO: META_PERIOD_RU` | Период работы, напр. `2024 — н.в.` |
| `TODO: META_PERIOD_EN` | То же на EN, напр. `2024 — present` |
| `TODO: CLIENT_URL` | URL клиента (или убрать `<a>`, оставить текст) |
| `TODO: HERO_IMG` | Путь к hero-картинке (`cases/...webp`) |

Секции кейса (`.section`) — копировать/удалять под нужное количество. Минимум: Задача + Процесс + Результат.

---

## Шаг 3 — Заполнить переводы

В конце файла найти блок `const translations = { ru: {...}, en: {...} }`.

Обязательные ключи (не трогать имена — они связаны с `data-i18n`):

```js
// ru и en — одинаковая структура
{
  back: 'Кейсы',             // не менять
  discuss: 'Обсудить',       // не менять
  footer_back: 'Все кейсы', // не менять
  footer_next_label: 'Следующий кейс', // не менять

  hero_desc: '...',          // описание на странице кейса

  dot_0: 'Обзор',   // подписи к точкам навигации (4 штуки)
  dot_1: 'Задача',
  dot_2: 'Процесс',
  dot_3: 'Результат',

  eyebrow_0: '...',  // надписи над заголовками секций
  title_0: '...',    // заголовки секций
  body_0: '...',     // тексты параграфов (нумеруются подряд)
  phase_0: '...',    // фазы (если есть список фаз)
  result_0: '...',   // результаты (если есть список результатов)

  meta_client: 'Клиент',        // не менять
  meta_period: 'Период',        // не менять
  meta_role: 'Роль',            // не менять
  meta_type: 'Тип',             // не менять
  meta_period_val: '2024 — н.в.' // ← менять под конкретный кейс
}
```

Каждый `data-i18n="body_N"` в HTML должен иметь соответствующий `body_N` в обоих языках.  
**Числа должны совпадать** — если в ru есть `body_5`, в en тоже должен быть `body_5`.

---

## Шаг 4 — Карточка в about-me.html

Найти секцию `<!-- Cases grid -->` (или `.cases-grid`) и добавить карточку:

```html
<div class="work-card" onclick="window.location='новый-кейс.html'" style="cursor:pointer;">
  <img src="cases/Cover.webp" alt="Название" class="work-card-img">
  <div class="work-card-body">
    <p class="work-card-eyebrow" data-i18n="case_N_sub">Тип кейса</p>
    <h3 class="work-card-title" data-i18n="case_N_title">Название</h3>
    <p class="work-card-desc" data-i18n="case_N_body">Краткое описание...</p>
  </div>
</div>
```

Где `N` = следующий порядковый номер (текущие: 0–5).

**Добавить переводы карточки** в `about-me.html` в оба языковых блока (`translations.ru` и `translations.en`):

```js
case_N_title: 'Название кейса',
case_N_sub: 'Тип / компания',
case_N_body: 'Одно–два предложения о проекте.',
```

---

## Шаг 5 — Цепочка footer-next

Текущая цепочка:
```
dna-match-tools → yourroots → dna-match → ydna → coderabbit → taara → [конец]
```

`taara.html` сейчас последний (нет `footer-next`). Если новый кейс добавляется в конец:

1. В `taara.html` найти `<footer class="case-footer">` и добавить ссылку на новый:
```html
<a href="новый-кейс.html" class="footer-next" aria-label="Следующий кейс" data-i18n-href="footer_next_label">Следующий кейс →</a>
```

2. В `новый-кейс.html` — `footer-next` не добавлять (он становится новым последним).

Если вставляете кейс в середину — обновить `footer-next` у того кейса, который должен вести на новый.

---

## Шаг 6 — Обновить CODEMAP.md

- Добавить файл в дерево файлов
- Добавить в цепочку навигации
- Добавить ассеты в таблицу инвентаря

---

## Что НЕ трогать без нужды

- CSS в `<style>` — он идентичен во всех кейсах, менять только если надо изменить дизайн всего сайта (придётся менять во всех файлах)
- Функцию `setLang` — она одинакова везде
- Структуру topbar — она должна быть одинаковой на всех страницах

---

## Частые ошибки

**SVG стрелка "назад" пропала при переключении языка**  
`data-i18n="back"` должен быть на `<span>` внутри `<a>`, не на самом `<a>`:
```html
<!-- ✅ правильно -->
<a href="about-me.html#cases" class="back-link">
  <svg>…</svg>
  <span data-i18n="back">Кейсы</span>
</a>
<!-- ❌ неправильно — setLang перезапишет textContent и сотрёт SVG -->
<a href="about-me.html#cases" class="back-link" data-i18n="back">…</a>
```

**Сломался десктоп грид (колонки в одну)**  
Мобильный стиль `grid-template-columns: 1fr` должен быть строго внутри `@media (max-width: 699px)`. Проверить, что закрывающая `}` медиа-запроса стоит после всех мобильных правил.

**JS-синтаксис после добавления переводов**  
Проверить файл перед деплоем:
```bash
node --check новый-кейс.html 2>&1 | head -5
# или
python3 -c "
import re, json
content = open('новый-кейс.html').read()
m = re.search(r'const translations = ({.*?});', content, re.DOTALL)
json.loads(m.group(1).replace(\"'\", '\"'))
print('JSON ok')
"
```

**og:image ссылается на несуществующий файл**  
После конвертации PNG → WebP обновить теги в `<head>`:
```html
<meta property="og:image" content="https://leri-ok.ru/cases/Файл.webp">
<meta name="twitter:image" content="https://leri-ok.ru/cases/Файл.webp">
```
