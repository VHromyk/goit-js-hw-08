# Домашнє завдання 8

## Тема: Галерея зображень з модальним вікном

---

## Завдання. Фотогалерея з lightbox

Виконано у файлі `js/gallery.js`

На сторінці є порожній список `<ul class="gallery">`. Є масив об'єктів `images`, де кожен об'єкт містить:

- `preview` — URL мініатюри для відображення в галереї
- `original` — URL оригінального зображення у великому розмірі
- `description` — опис зображення (alt-текст)

### Що реалізовано

**1. Динамічна генерація розмітки галереї**

За допомогою `map` та `join` генерується список `<li>` елементів з посиланнями та зображеннями, які вставляються у DOM через `insertAdjacentHTML`.

```js
const galleryList = images
  .map(
    (item) =>
      `<li class="gallery-item">
  <a class="gallery-link" href="${item.original}">
    <img
      class="gallery-image"
      src="${item.preview}"
      data-source="${item.original}"
      alt="${item.description}"
    />
  </a>
</li>`
  )
  .join("");

containerRef.insertAdjacentHTML("beforeend", galleryList);
```

**2. Делегування подій**

Обробник кліку підписано на батьківський контейнер `.gallery` (делегування), а не на кожне зображення окремо.

```js
containerRef.addEventListener("click", handleClick);
```

**3. Модальне вікно через basicLightbox**

При кліку на зображення відкривається модальне вікно з оригінальним фото за допомогою бібліотеки [basicLightbox](https://basiclightbox.electerious.com/).

```js
function createModal(url, alt) {
  const instance = basicLightbox.create(`
    <div class="modal">
      <div class="modal-wrapper">
        <img class="inside-img" src="${url}" alt="${alt}" />
      </div>
    </div>
  `);
  instance.show();
}
```

**4. Адаптивна сітка**

Галерея реалізована на CSS Grid з брейкпоінтами:

| Ширина екрану | Кількість колонок |
|---|---|
| < 768px | 1 |
| 768px – 1199px | 2 |
| ≥ 1200px | 3 |

---

## Технології та бібліотеки

- **Vanilla JavaScript** — без фреймворків
- **basicLightbox v5.0.4** — легка бібліотека для модальних вікон (підключена через CDN)
- **CSS Grid** — адаптивна сітка галереї

---

## Структура проекту

```
08-goit-js-hw-08/
├── index.html
├── css/
│   └── styles.css
└── js/
    └── gallery.js
```

---

## На що звертає увагу ментор при перевірці

- Розмітка генерується динамічно через `map` та `join`
- Вставка у DOM відбувається через `insertAdjacentHTML`
- Кожне `<img>` має атрибути `src`, `data-source` та `alt`
- Обробник підписано на контейнер (делегування подій)
- Стандартна поведінка посилання заблокована через `event.preventDefault()`
- При кліку на порожню область галереї модальне вікно не відкривається
- Модальне вікно відкривається з оригінальним зображенням через бібліотеку basicLightbox