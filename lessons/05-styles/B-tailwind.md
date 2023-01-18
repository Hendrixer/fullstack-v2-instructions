## Tailwind CSS Configuration

Our `tailwind.config.js` looks like this:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx}", // Note the addition of the `app` directory.
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ["var(--font-inter)"],
      },
    },
  },
  plugins: [],
};
```

We then need to add directives to our global CSS file

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Tailwind CSS requires PostCSS

In this lesson, Tailwind CSS is configured, but is not working propery. This is because Tailwind CSS requires PostCSS. Scott fixes this issue in the **SidebarLink Component** lesson. If you want to fix the issue now, run these two commands:

```bash
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

> ✔️ Code Checkpoint: The current code for the application can be found on the [layouts branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/layouts). This checkpoint does not include the Tailwind CSS fix.
