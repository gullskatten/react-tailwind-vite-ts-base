# react-tailwind-vite-ts-base
My base recipe for configuring a new React project with ESLint + Prettier, Tailwind, Vite, TypeScript

Make sure you are using node version ^20 (run `node -v`).

[Tailwindcss + Vite Reference Docs](https://tailwindcss.com/docs/guides/vite)

[Vite: create-vite Reference Docs](https://github.com/vitejs/vite/tree/main/packages/create-vite)

## Initialize a [Vite react-ts project](https://vitejs.dev/guide/#scaffolding-your-first-vite-project)

```bash
npm create vite@latest my-project -- --template react && cd my-project
```

Replace `my-project` with your project name.

### Recommended naming convention for a project
- [kebab case](https://en.wikipedia.org/wiki/Letter_case#Kebab_case) (lower-case words, use hyphens instead of spaces)
- Examples: `quick-brown-fox`, `lazy-dog`, `gullskatten-api` 


## Install dependencies

```bash
npm install -D tailwindcss postcss autoprefixer prettier prettier-plugin-tailwindcss
```

- [`tailwindcss`](https://tailwindcss.com): Utility css classname library used for styling our components
- [`postcss, autoprefixer`](https://postcss.org/): Automatically adds vendor prefixes (-o-, -moz-) to css output
- [`prettier prettier-plugin-tailwindcss`](https://tailwindcss.com/blog/automatic-class-sorting-with-prettier): Sort classnames based on recommended sorting order

## Initialize tailwindcss

```bash
npx tailwindcss init -p
```

This will create the required config files for `tailwindcss` (`tailwind.config.js`) and `postcss` (`postcss.config.js`).

### Configure template paths in `tailwind.config.js`

Open `tailwind.config.js` and add the following:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'], // Indicates which files in our project uses tailwind utility classes
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### Enable @tailwind components, base and utilities in `index.css`

Open `./src/index.css` and add the `@tailwind` lines below at the top of this file. (Optional): You can also remove any other classes/styling except the `:root` class.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Rest of css classes .. */
:root {
 ...
}
```

## Add a [Prettier Configuration](https://prettier.io/docs/en/configuration)

ESLint has already been installed as a dependency by `create-vite`. However, I prefer using Prettier for formatting rules, and ESLint only for tracing errors/warnings in code. To make sure that Prettier and ESLint are not conflicting, we'll add a package which resolves any conflicting rules later on. First, let's set up Prettier.

1. Create a file named `.prettierrc` in your project root (Prettier configuration file)

```bash
touch .prettierrc
```

2. Open `.prettierrc`, and add the following contents:

```json
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "plugins": ["prettier-plugin-tailwindcss"],
  "bracketSpacing": true,
  "bracketSameLine": true
}
```

Note: I added some more formatting rules to the configuration file. You can customize the settings in this configuration file to match your preference - the only important part is the `"plugins"` key.

3. Add [eslint-config for prettier](https://github.com/prettier/eslint-config-prettier): Make sure ESLint does not conflict with Prettier (for formatting) by installing the following package:

```bash
npm install -D eslint-plugin-prettier eslint-config-prettier
```

4. Add the `prettier` plugin to the eslint-config

Open `.eslintrc.cjs` and add `plugin:prettier/recommended` as the last element in the `"extends": []` array:

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended",
    "plugin:prettier/recommended" // Add this as the last element
  ]
}
...
```

See [Prettier recommended config](https://github.com/prettier/eslint-plugin-prettier#recommended-configuration) for more info.

5. Your project is now correctly configured, and the development server can now be started.

```
npm run dev
```
