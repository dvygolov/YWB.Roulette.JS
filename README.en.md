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

[Support this project](https://yellowweb.top/donate)

# YWB.Roulette.JS

[Русский](README.md) | [English](README.en.md)

A landing-page discount wheel with an initialization API similar to YWB Doors, GiftBoxes and 3from5. Version **1.0.0**. No jQuery or other dependencies.

[Live demo and documentation (Russian)](https://yellow-scripts.pages.dev/scripts/roulette/) | [Yellow Web](https://yellowweb.top/)

![Roulette in the included index.html demo (Russian labels)](screenshot.png)

## Installation

Copy `ywbroulette.js`, `ywbroulette.css` and `prizewheel.png` to your website. The wheel container and order form must be separate elements; do not place the form inside the wheel container.

```html
<link rel="stylesheet" href="ywbroulette.css">

<div id="roulette"></div>
<form id="order" action="/order.php" method="post">
  <input name="name" autocomplete="name" required>
  <input name="phone" type="tel" autocomplete="tel" required>
  <button type="submit">Order</button>
</form>

<script src="ywbroulette.js"></script>
<script>
  const roulette = initRoulette({
    selectors: { roulette: '#roulette', form: '#order' },
    texts: {
      title: 'Reveal your discount',
      button: 'Spin',
      popupTitle: 'Your discount: 50%',
      popupText: 'Your discount is available in the order form.',
      confirm: 'Claim discount'
    },
    image: 'prizewheel.png',
    duration: 4500,
    stopAngle: 67.5,
    onResult: ({ angle }) => console.log('Result', angle),
    onComplete: () => console.log('Form revealed')
  });
</script>
```

Call `initRoulette` after both elements exist in the DOM. CSS, JavaScript and image paths in this example are relative to the HTML page. A server-side form handler is not included.

## Options and API

| Option | Default | Description |
| --- | --- | --- |
| `selectors.roulette` | Required | CSS selector or DOM element for the wheel container |
| `selectors.form` | Required | CSS selector or DOM element for the form or its wrapper |
| `texts` | Russian labels | Heading, spin button, result title and message, confirmation button |
| `image` | `prizewheel.png` | Wheel image |
| `duration` | `4500` | Animation duration in milliseconds, minimum 0 |
| `stopAngle` | `67.5` | Final angle in degrees after five full turns |
| `onResult` | Not set | Callback after the wheel stops and the result opens |
| `onComplete` | Not set | Callback after confirmation and revealing the form |

The returned instance exposes `spin()`, `destroy()` and a `state` property: `ready`, `spinning`, `result`, `complete`. Calling `initRoulette` again for the same container returns the existing instance. `destroy()` cancels the timer, removes the widget and restores the original container markup and form visibility. To start a new round, call `initRoulette` again after `destroy()`.

Repeated clicks while spinning are ignored. The button supports keyboard activation. Escape in the result dialog confirms the result and reveals the form. Focus moves to the first field. Animation is skipped when `prefers-reduced-motion` is enabled.

The `texts` option customizes the visible headings and buttons shown above. The image alt text and spinning status remain in Russian in version 1.0.0.

## Result and Artwork

The discount is predetermined, not random. With the included image, `67.5` places the 50% sector under the top pointer. If you replace the image or change the angle, match the result text to the displayed sector. Your server must validate the discount applied to an order.

Rights to the artwork belong to its respective rights holders; no separate license for it is granted here.

## Wheels

Choose a design, copy the `wheels` directory and set `image` and `stopAngle`. Each preview lists the sector that will stop under the top pointer. Configure the result message separately through `texts`.

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
| 50% | Light unlabeled sector |

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
| Green unlabeled sector | 50% |

| bonus-spinner | prize-gold |
| --- | --- |
| ![bonus-spinner](wheels/bonus-spinner.png) | ![prize-gold](wheels/prize-gold.png) |
| [bonus-spinner.png](wheels/bonus-spinner.png) | [prize-gold.png](wheels/prize-gold.png) |
| `stopAngle: 315` | `stopAngle: 315` |
| 50 FS | 50 (no unit) |

## Trying the Example

Open `index.html` in a browser. The included example disables form submission with an explicitly marked `submit` handler. Remove that handler and set your own form `action` when deploying to a live landing page.

The example has been tested at mobile and desktop widths, including wheel animation, repeated activation, result confirmation, keyboard controls, focus transfer, reinitialization and instance destruction.
