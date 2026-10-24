# Territory Drawer

Интерактивный симулятор: обводите область на спутниковой карте — территория переходит к «Україна» или «Росія».
Статический сайт без сборки: открывается как есть, всё приложение в `index.html`.

## Файлы
| Файл | Зачем |
|---|---|
| `index.html` | приложение целиком (HTML + CSS + JS + границы Украины) |
| `_headers` | заголовки безопасности (CSP и др.) — работают на Cloudflare Pages |
| `.nojekyll` | чтобы GitHub Pages не обрабатывал файлы Jekyll'ом |
| `404.html` | настоящая страница 404 (без неё Cloudflare Pages отдаёт `index.html` на любой адрес) |
| `robots.txt` | разрешает индексацию |
| `site.webmanifest`, `icon-*.png`, `apple-touch-icon.png` | иконки и «Добавить на главный экран» |
| `og.png` | картинка для превью ссылки в соцсетях (1200×630) |

## Публикация: GitHub → Cloudflare Pages
1. Создайте репозиторий на GitHub и загрузите в его корень **все** файлы из папки (включая `.nojekyll` и `_headers`).
2. Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git** → выберите репозиторий.
3. Настройки сборки: *Framework preset* — **None**, *Build command* — пусто, *Build output directory* — `/`. Нажмите **Save and Deploy**.
4. Через минуту сайт будет на `https://<имя-проекта>.pages.dev`. Свой домен: проект → **Custom domains**.

Каждый `git push` в основную ветку публикуется автоматически.
GitHub Pages включать не обязательно: Cloudflare Pages берёт файлы прямо из репозитория. Если включите и его (*Settings → Pages → Deploy from a branch → main / root*), получится вторая копия на `github.io`, но `_headers` там не действует.

## Когда появится адрес сайта
Добавьте в `<head>` файла `index.html` (подставьте свой адрес) и замените `twitter:card` на `summary_large_image`:
```html
<link rel="canonical" href="https://ВАШ-АДРЕС/">
<meta property="og:url" content="https://ВАШ-АДРЕС/">
<meta property="og:image" content="https://ВАШ-АДРЕС/og.png">
<meta name="twitter:image" content="https://ВАШ-АДРЕС/og.png">
```

## Проверка после деплоя
- Откройте сайт, нажмите F12 → Console: не должно быть ошибок `Content Security Policy`. Если что-то не грузится из-за CSP — временно удалите строку `Content-Security-Policy` из `_headers` и сообщите, что блокировалось.
- Нарисуйте контур мышью и пальцем, подвигайте карту правой кнопкой мыши, нажмите «⬇» (сохранение картинки).

## Заметки
- Не включайте на Cloudflare *Web Analytics* и *Rocket Loader*: первый блокируется CSP (сайт заявляет «без отслеживания»), второй может ломать встроенные скрипты (в `index.html` стоит `data-cfasync="false"`).
- Спутниковые тайлы Esri подключены без ключа. Для коммерческого проекта проверьте условия Esri (обычно нужен ключ ArcGIS).
- Источники: Leaflet (BSD-2), контур России — Natural Earth (общественное достояние), границы Украины — ваш набор административных границ (формат COD-AB, `valid_on` 2025-09-01): проверьте лицензию набора на странице-источнике (часто CC BY-IGO — тогда нужно указать источник).
- Если репозиторий публичный, добавьте файл `LICENSE` по своему выбору.
