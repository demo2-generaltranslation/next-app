<!-- BEGIN:nextjs-agent-rules -->
# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.
<!-- END:nextjs-agent-rules -->

<!-- GT I18N RULES START -->

- **gt-next**: ^6.14.2
- **gt**: v2.10.2

# General Translation (GT) Internationalization Rules

This project is using [General Translation](https://generaltranslation.com/docs/overview.md) for internationalization (i18n) and translations. General Translation is a developer-first localization stack, built for the world's best engineering teams to ship apps in every language with ease.

## Configuration

The General Translation configuration file is called `gt.config.json`. It is usually located in the root or src directory of a project.

```json
{
  "defaultLocale": "en",
  "locales": ["es", "fr", "de"],
  "files": {
    "json": {
      "include": ["./**/[locale]/*.json"]
    }
  }
}
```

The API reference for the config file can be found at <https://generaltranslation.com/docs/cli/reference/config.md>.

## Translation

Run `npx gt translate` to create translation files for your project. You must have an API key to do this.

## Documentation

<https://generaltranslation.com/llms.txt>


# gt-next

This project is using the `gt-next` internationalization library for Next.js App Router.

## gt-next setup

- `GTProvider` must wrap the app in the root layout to provide translation context.
- The `withGTConfig()` plugin wraps `next.config` in the Next.js config file.
- (optional) `createNextMiddleware()` is used in `proxy.ts` for automatic locale routing.

## Translating JSX

`gt-next` uses the `<T>` component for translation.

Pass JSX content as the direct children of `<T>` to translate it. Children of `<T>` must be static — no JS expressions or variables directly inside.

```jsx
import { T } from 'gt-next';

<T>
  <h1>Welcome to our store</h1>
  <p>
    Browse our <a href='/products'>latest products</a> and find something you
    love.
  </p>
</T>;
```

You can also add a `context` prop to `<T>` to give context to the translator. For example:

```jsx
import { T } from 'gt-next';

<T context="Cookies as in web cookies">
  View your <a href="/cookies">Cookies</a>
</T>;
```

## Translating simple strings

Use the `gt` function returned by the `useGT()` hook to translate strings directly. Invoke `useGT()` in synchronous components or `await getGT()` in async components only.

```js
import { useGT } from 'gt-next';
const gt = useGT();
gt('Hello, world!'); // returns "Hola, mundo"
```

```js
import { getGT } from 'gt-next/server';
const gt = await getGT(); // use await version in async components only
gt('Hello, world!');
```

- Just like with the children of the `<T>` component, all strings passed to `gt()` must be static string literals. No variables or template literals.

## Translating shared or out-of-scope strings

Use `msg()` to register strings for translation, and `useMessages()` to translate them. `const m = useMessages()` should be used equivalently to `const gt = useGT()`.

```js
import { msg, useMessages } from 'gt-next';

const greeting = msg('Hello, world!');

export default function Greeting() {
  const m = useMessages();
  return <p>{m(greeting)}</p>;
}
```

- All strings passed to `msg()` must be static string literals. No variables or template literals.
- Use the equivalent `await getMessages()` for async components.
- `useMessages()` / `getMessages()` take no arguments.

## Dynamic content inside `<T>`

Use variable components for dynamic values inside `<T>`:

- `<Var>{value}</Var>` — variables (strings, numbers, etc.)
- `<Num>{value}</Num>` — formatted numbers
- `<Currency>{value}</Currency>` — formatted currency
- `<DateTime>{value}</DateTime>` — formatted dates/times

```jsx
import { T, Var, Num } from 'gt-next';

<T>
  <Var>{userName}</Var> ordered <Num>{itemCount}</Num> items.
</T>;
```

## Utility hooks

### `useLocale()`

`useLocale` returns the user's current language, as a BCP 47 locale tag.

```js
import { useLocale } from 'gt-next'

const locale = useLocale(); // "en-US"
```

## Quickstart

See <https://generaltranslation.com/docs/next.md>

<!-- GT I18N RULES END -->
