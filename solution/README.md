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


### 


## Design

### Fonts

For the purposed of this exercise we will use the `system-ui` font stack.

**Question:** If we required a custom typeface, what would be your approach? What are the potential risks of using a custom typeface?

### Colors

| Color       | Hex       |
| ----------- | --------- |
| Base        | `#003453` |
| Border      | `#dde1e3` |
| Hover/Focus | `#d8f7fb` |

**Question:** What would be your approach to organise colors in a larger palette? How would you maintain colors as part of a wider design system?
