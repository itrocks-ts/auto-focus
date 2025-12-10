[![npm version](https://img.shields.io/npm/v/@itrocks/auto-focus?logo=npm)](https://www.npmjs.org/package/@itrocks/auto-focus)
[![npm downloads](https://img.shields.io/npm/dm/@itrocks/auto-focus)](https://www.npmjs.org/package/@itrocks/auto-focus)
[![GitHub](https://img.shields.io/github/last-commit/itrocks-ts/auto-focus?color=2dba4e&label=commit&logo=github)](https://github.com/itrocks-ts/auto-focus)
[![issues](https://img.shields.io/github/issues/itrocks-ts/auto-focus)](https://github.com/itrocks-ts/auto-focus/issues)
[![discord](https://img.shields.io/discord/1314141024020467782?color=7289da&label=discord&logo=discord&logoColor=white)](https://25.re/ditr)

# auto-focus

Sets focus on the first form field within a specified DOM element.

*This documentation was written by an artificial intelligence and may contain errors or approximations.
It has not yet been fully reviewed by a human. If anything seems unclear or incomplete,
please feel free to contact the author of this package.*

## Installation

```bash
npm i @itrocks/auto-focus
```

## Usage

`@itrocks/auto-focus` exports a single helper function `autoFocus`.

You give it a reference to a `HTMLFormElement`; it will look for the
first eligible input inside that form and move the browser focus to it.

An input is considered eligible when:

- it is an `input`, `select` or `textarea` element,
- it is **not** of type `submit`,
- it does **not** have the `readonly` attribute,
- it is visible (`display` is not `none` and `visibility` is not
  `hidden`).

### Minimal example

```ts
import { autoFocus } from '@itrocks/auto-focus'

// After your form is rendered in the DOM
const form = document.querySelector<HTMLFormElement>('#login-form')

if (form) {
  autoFocus(form)
}
```

Whenever this code runs, the first non‑readonly, visible field in the
`#login-form` will receive focus (for example the `email` input).

### Example with a framework / SPA

You can call `autoFocus` in any place where you have access to the
rendered form element, for example after mounting a component.

```ts
import { autoFocus } from '@itrocks/auto-focus'

// Example: generic helper that you can call from your UI framework
export function focusFirstField (form: HTMLFormElement | null) {
  if (form) {
    autoFocus(form)
  }
}
```

In a framework that gives you a form ref (React, Vue, Svelte, etc.),
you can wire this helper to the appropriate lifecycle hook so that the
first meaningful field is focused as soon as the dialog or page opens.

## API

### `function autoFocus(element: HTMLFormElement): void`

Moves the browser focus to the first relevant field within the given
form.

#### Parameters

- `element: HTMLFormElement` – the form whose fields should be
  inspected. All descendants matching `input`, `select` and `textarea`
  are considered.

#### Behaviour

1. The helper lists all `input`, `select` and `textarea` elements in
   document order within the provided form.
2. It filters out elements that:
   - are of type `submit`,
   - have a `readonly` attribute,
   - are hidden via CSS (`display: none` or `visibility: hidden`).
3. If at least one eligible element remains, it calls `.focus()` on the
   first one.

If no eligible element is found, nothing happens.

#### Return value

- `void` – the function performs a side effect (focusing the element)
  and does not return anything.

## Typical use cases

- **Login / authentication forms** – automatically focus the username or
  email field when the page or modal opens.
- **Search bars** – give immediate focus to the search input when
  navigating to a search page.
- **Modal or slide‑in forms** – when opening a dialog that contains a
  form (new record, quick edit, filter panel, etc.), call `autoFocus`
  once the dialog content is attached to the DOM so the user can start
  typing right away.
- **Wizard steps / multi‑page forms** – at each step transition, focus
  the first relevant field of the new step to keep keyboard navigation
  fluid.
- **Accessibility / UX polish** – ensure keyboard users and screen
  reader users land on the primary input instead of having to tab
  through buttons or hidden fields.
