# UI Assessment Solution

## Task

### How the label the navigation to provide context for accessibility technologies to understand it is a "Main navigation"?

#### Using `aria-label`

A label can be applied to `<nav>` with `aria-label` for the accessibility tree to provide context for the navigation. This is read by screen-readers when a user selects the `<nav>`. 

```html
<nav
  aria-label="Main navigation"
>
  ...
</nav>
```

#### Using `aria-labelledby`

The preferred approach is to use a `<h2>` (or appropriate heading level) with and `id` which is associated to the `<nav>` with `aria-labelledby`.

This is a more robust approach as screen-reader users are more likely to navigate a page via headings (source: [Screen Reader User Survey](https://webaim.org/projects/screenreadersurvey8/)).

By using a heading the `<nav>` has the appropriate label, and there is a heading for users to navigate to and provide context before interacting with the `<nav>` itself.

```html
<h2
  id="heading"
  class="visually-hidden"
>
  Main navigation
</h2>
<nav
  aria-labelledby="heading"
>
  ...
</nav>
```

### How to markup a navigation for accessibility?

The navigation should be wrapped in a `<nav>` element with an unordered list `<ul>` that contains a list item `<li>` and anchor `<a>` for each link.

```html
<nav>
  <ul>
    <li>
      <a href="#">...</a>
    </li>
  </ul>
</nav>
```

### How to represent an active link in the navigation?

The active page link in the navigation should be provided with the [`aria-current`](https://www.w3.org/TR/wai-aria-1.1/#aria-current) attribute. Screen readers should announce "Current page" for the `<a>` element with `aria-current="page"`.

```html
<a aria-current="page" href="...">...</a>
```

### How would you approach including the SVG icons?

#### Using `<img>`

As the SVG icons do not require custom styling for different states (`:hover`, `:focus` etc.) it is appropriate to use an `<img>` element, at least for the exercise.

However, there are issues with this approach which come at scale that should be communicated:

1. Increase network requests.
2. Requires an `alt` attribute to be populated, either with content that describes the icon (if applicable) or when decorative (in this case) it's appropriate to use `alt=""` to hide the image from the accessibility tree.
3. Image source will need to be updated with `fill` color.

```html
<img
  alt=""
  height="1.5rem"
  src="house.svg"
  width="1.5rem"
/>
```

The image will need to have a `height` and `width` set either as attributes on `<img>` or in CSS.

Ideally these should use `1.5rem` so the SVG scales as root font-size is increase for users who zoom their interface.

```css
img {
  display: inline-block;
  height: 1.5rem; /* equals 24px */
  width: 1.5rem; /* equals 24px */
}
```

Linked SVG assets will need to be updated to set the `fill` attribute. Linked SVG images in the `<img>` element cannot be styled with CSS.

```html
<svg
  fill="#003453"
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 24 24"
>
  ...
</svg>
```

#### Using `<svg>`

The source of each icon can be included on the DOM and a HTML node. This does not require a network request to resolve, served with initial HTML request, and can be styled with CSS.

```html
<svg
  focusable="false"
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 24 24"
>
  <path d="M12.57 2.18l.1.08 10 9a1 1 0 01-1.24 1.56l-.1-.08-1.33-1.2V20a2 2 0 01-2 2h-3a1 1 0 01-1-1v-4a1 1 0 00-1-1h-2a1 1 0 00-1 1v4a1 1 0 01-1 1H6a2 2 0 01-2-2v-8.46l-1.33 1.2a1 1 0 01-1.32.02l-.1-.1a1 1 0 010-1.31l.08-.1 10-9a1 1 0 011.24-.07z" />
</svg>
```

Styling for the SVG icons should allow for the `fill` to inherit the `currentColor`.

```css
svg {
  display: inline-block;
  fill: currentColor; /* inherits root color #003453 */
  height: 1.5rem; /* equals 24px */
  width: 1.5rem; /* equals 24px */
}
```

## Style

### Reset

#### Border Box

In order to have the `border` properties apply consistently across browsers it is best to set `box-sizing: border-box` on all applicable elements.

To easily achieve this it can be set with the wildcard (`*`) selector:

```css
* {
  box-sizing: border-box;
}
```

#### Font rendering

To have consistent font rendering across browsers it is best to srt the following properties on the `:root`, `html`, or `body` selector:

1. `font-family` in this case `font-family: system-ui` (or the full font-stack `font-family: -apple-system, BlinkMacSystemFont, avenir next, avenir, helvetica neue, helvetica, Ubuntu, roboto, noto, segoe ui, arial, sans-serif;`)
2. `-webkit-text-size-adjust: 100%` to handle iOS properly resizing the text when the browser page is zoomed
3. `font-smoothing` to ensure `font-weight` is stylistically the same between browsers

```css
html {
  font-family: system-ui;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}
```

### Task

#### Container

Wrapping the navigation is a container which limits the width of the content extending past `640px`.

Best practice is to use `rem` values to ensure that the `max-width` property value also scales when the root `font-size` is increased.

```css
.container {
  margin-left: auto;
  margin-right: auto;
  max-width: 40rem; /* equals 640px */
}
```

#### Hover/Focus

Apply `background-color` and `border-color`.

1. `background-color` instead of `background` for more explicit CSS styles to avoid overides of other background properties (e.g. `background-position`, `background-size` etc.).
2. `border-color` the border should be set on the link and be made visible with `border-color` instead of setting all border properties on `:hover`/`:focus` to avoid content shifting.

```css
a:hover,
a:focus {
  background-color: var(--background-hover); /* or color equivalent */
  border-color: var(--color-base); /* or color equivalent */
}
```

#### Current page

To style the current page's link use the `[aria-current="page"]` selector, or custom class (e.g. `.active`) which is applied with logic with the `aria-current="page"` attribute.

```css
a[aria-current="page"] {
  border-color: var(--color-base); /* or color equivalent */
  font-weight: 600; /* or bold */
}
```

#### Visually hidden content

Ensure visually-hidden content (in this case the `<h2>`) is still decernable by screen-readers. This may also be `.sr-only`, `.screen-reader`, `.hide-visually` etc.

```css
.visually-hidden {
  border: none;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
```

**Please note:** using `display: none` or `visibility: hidden` removes the content from the accessibility tree and means it cannot be read by assistive technologies like screen-readers.

## Design

### Fonts

#### If we required a custom typeface, what would be your approach? What are the potential risks of using a custom typeface?

**Possible Answer(s):**

- Flash of un-styled text (FOUT) when the `font-face` is loading.
- Decreased performance as `font-face` is render-blocking unless `font-display` property is set.
- Stylistic rendering, custom typeface fonts require alignment for use on the web, print fonts may not render as expected (also between browsers) and lead to increased or misaligned whitespace.
- Require fallback font-stack in the case that a browser or user has blocked custom fonts to ensure content renders as expected.

### Colors

#### What would be your approach to organise colors in a larger palette? How would you maintain colors as part of a wider design system?

**Possible Answer(s):** 

- Use SASS/SCSS variables or CSS custom properties to define colors in a single location for use throughout UI
- Atomic CSS classes which set `color: value` only to ensure CSS is DRY (don't repeat yourself).
- Abstract naming convention for colors e.g. `blue-500`, `brand-base`, `primary-default` etc.
- Define color scales e.g. Material Designs color scale `100` - `900`.