```text
    _            __     __  _ _             __          __  _
   | |           \ \   / / | | |            \ \        / / | |
   | |__  _   _   \ \_/ /__| | | _____      _\ \  /\  / /__| |__
   | '_ \| | | |   \   / _ \ | |/ _ \ \ /\ / /\ \/  \/ / _ \ '_ \
   | |_) | |_| |    | |  __/ | | (_) \ V  V /  \  /\  /  __/ |_) |
   |_.__/ \__, |    |_|\___|_|_|\___/ \_/\_/    \/  \/ \___|_.__/
           __/ |
          |___/             https://yellowweb.top

If you like this script, PLEASE DONATE!
```

[Поддержать проект](https://yellowweb.top/donate)

# YWB.Roulette.JS

[Русский](README.md) | [English](README.en.md)

Рулетка для лендинга с подключением в стиле YWB Doors, GiftBoxes и 3from5. Версия **1.0.0**. Без jQuery и других зависимостей.

[Живой пример и описание](https://yellow-scripts.pages.dev/scripts/roulette/) · [Жёлтый Веб](https://yellowweb.top/)

![Рулетка в комплектном примере index.html](screenshot.png)

## Подключение

Скопируйте `ywbroulette.js`, `ywbroulette.css` и `prizewheel.png` на свой сайт. Контейнер рулетки и форма должны быть отдельными элементами; форма не должна находиться внутри контейнера рулетки.

```html
<link rel="stylesheet" href="ywbroulette.css">

<div id="roulette"></div>
<form id="order" action="/order.php" method="post">
  <input name="name" autocomplete="name" required>
  <input name="phone" type="tel" autocomplete="tel" required>
  <button type="submit">Заказать</button>
</form>

<script src="ywbroulette.js"></script>
<script>
  const roulette = initRoulette({
    selectors: { roulette: '#roulette', form: '#order' },
    texts: {
      title: 'Откройте свою скидку',
      button: 'Крутить',
      popupTitle: 'Ваша скидка — 50%',
      popupText: 'Скидка доступна в форме заказа.',
      confirm: 'Получить скидку'
    },
    image: 'prizewheel.png',
    duration: 4500,
    stopAngle: 67.5,
    onResult: ({ angle }) => console.log('Результат', angle),
    onComplete: () => console.log('Форма открыта')
  });
</script>
```

Вызывайте `initRoulette` после появления обоих элементов в DOM. Пути CSS, JS и картинки в примере относительны к HTML-странице. Серверный обработчик заявок в комплект не входит.

## Настройки и API

| Параметр | Значение по умолчанию | Назначение |
| --- | --- | --- |
| `selectors.roulette` | Обязателен | CSS-селектор или DOM-элемент контейнера игры |
| `selectors.form` | Обязателен | CSS-селектор или DOM-элемент формы/блока с формой |
| `texts` | Русские подписи | Заголовок, кнопка, текст результата и подтверждение |
| `image` | `prizewheel.png` | Изображение колеса |
| `duration` | `4500` | Длительность в миллисекундах, минимум 0 |
| `stopAngle` | `67.5` | Конечный угол в градусах после пяти оборотов |
| `onResult` | Не задан | Вызов после остановки и открытия результата |
| `onComplete` | Не задан | Вызов после подтверждения и открытия формы |

Экземпляр возвращает `spin()`, `destroy()` и свойство `state`: `ready`, `spinning`, `result`, `complete`. Повторный `initRoulette` для того же контейнера возвращает существующий экземпляр. `destroy()` отменяет таймер, убирает интерфейс и восстанавливает исходную разметку контейнера и видимость формы. Для нового розыгрыша после `destroy()` вызовите `initRoulette` заново.

Повторные клики во время вращения игнорируются. Кнопка работает с клавиатуры. Escape в окне результата подтверждает результат и открывает форму. Фокус переходит в первое поле. При `prefers-reduced-motion` вращение пропускается.

## Результат и графика

Это заданная скидка, а не случайная лотерея. У стандартного изображения угол `67.5` останавливает сектор 50% под верхним указателем. При замене изображения или угла согласуйте текст результата с нарисованным сектором. Размер скидки в заказе должен проверяться вашим сервером.

Права на графику принадлежат её правообладателям; отдельная лицензия на неё здесь не предоставляется.

## Колёса

Выберите дизайн, скопируйте папку `wheels` и задайте `image` и `stopAngle`. Под каждым превью указан сектор, который окажется под верхней стрелкой. Текст результата задаётся отдельно через `texts`.

```js
const roulette = initRoulette({
  selectors: { roulette: '#roulette', form: '#order' },
  image: 'wheels/multicolor.png',
  stopAngle: 315
});
```

| multicolor | gradient |
| --- | --- |
| ![multicolor](wheels/multicolor.png) | ![gradient](wheels/gradient.png) |
| [multicolor.png](wheels/multicolor.png) | [gradient.png](wheels/gradient.png) |
| `stopAngle: 315` | `stopAngle: 0` |
| 50% | 50% |

| blue-yellow | red-yellow |
| --- | --- |
| ![blue-yellow](wheels/blue-yellow.png) | ![red-yellow](wheels/red-yellow.png) |
| [blue-yellow.png](wheels/blue-yellow.png) | [red-yellow.png](wheels/red-yellow.png) |
| `stopAngle: 180` | `stopAngle: 67.5` |
| 50% | 50% |

| coral-turquoise | pastel |
| --- | --- |
| ![coral-turquoise](wheels/coral-turquoise.png) | ![pastel](wheels/pastel.png) |
| [coral-turquoise.png](wheels/coral-turquoise.png) | [pastel.png](wheels/pastel.png) |
| `stopAngle: 90` | `stopAngle: 0` |
| 50% | Светлый сектор без надписи |

| discounts | jackpot |
| --- | --- |
| ![discounts](wheels/discounts.png) | ![jackpot](wheels/jackpot.png) |
| [discounts.png](wheels/discounts.png) | [jackpot.png](wheels/jackpot.png) |
| `stopAngle: 135` | `stopAngle: 330` |
| −50% | Jackpot |

| colors | green-discount |
| --- | --- |
| ![colors](wheels/colors.png) | ![green-discount](wheels/green-discount.png) |
| [colors.png](wheels/colors.png) | [green-discount.png](wheels/green-discount.png) |
| `stopAngle: 0` | `stopAngle: 315` |
| Зелёный сектор без надписи | 50% |

| bonus-spinner | prize-gold |
| --- | --- |
| ![bonus-spinner](wheels/bonus-spinner.png) | ![prize-gold](wheels/prize-gold.png) |
| [bonus-spinner.png](wheels/bonus-spinner.png) | [prize-gold.png](wheels/prize-gold.png) |
| `stopAngle: 315` | `stopAngle: 315` |
| 50 FS | 50 (без единицы измерения) |

## Проверка примера

Откройте `index.html` в браузере. В комплектном примере отправка формы отключена явно указанным обработчиком `submit`. Удалите его при установке на рабочий лендинг и укажите свой `action`.

Проверены мобильная и десктопная ширина, движение колеса, повторный запуск, подтверждение результата, управление клавиатурой, переход фокуса, повторная инициализация и уничтожение экземпляра.
