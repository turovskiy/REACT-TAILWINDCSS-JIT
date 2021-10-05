# Установка tailwindCSS в режиме JIT для create-react-app

Начните с создания нового проекта Create React App, если у вас его еще нет. Самый распространенный подход - использовать [Create React App](https://github.com/facebook/create-react-app).

## Установка для create-react-app

Создаем новое приложение

```bash
npx create-react-app my-newapp
```

И заходим в него

```bash
cd my-project
```

Устанавливаем Tailwind его одноранговые зависимости, используя npm:

```bash
npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
```

Поскольку Create React App не позволяет вам изначально переопределить конфигурацию PostCSS, нам также необходимо установить CRACO, чтобы иметь возможность настраивать Tailwind:

```bash
npm install @craco/craco
```

После его установки обновите ваши scripts в файле package.json, чтобы использовать craco вместо react-scripts для всех скриптов, кроме eject:

```json
{
    // ...
    "scripts": {
     "start": "cross-env TAILWIND_MODE=watch craco start",
     "build": "craco build",
     "test": "craco test",
      "eject": "react-scripts eject"
    },
  }
```

Затем создайте craco.config.js в корне нашего проекта и добавьте tailwindcss и autoprefixer как плагины PostCSS:

```jsx
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

Теперь настроим режим jit

Устанпвливаем cross-env

```bash
npm i cross-env
```

Затем сгенерируйте свои(й) tailwind.config.js файл:npx tailwindcss-cli@latest init

```bash
npx tailwindcss-cli@latest init
```

Это создаст минимальный файл tailwind.config.js в корне вашего проекта

```jsx
// tailwind.config.js
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

включаем jit и настраиваем purge

```jsx
// tailwind.config.js
module.exports = {
  mode: "jit",
  purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

Откройте файл ./src/index.css, который Create React App генерирует для вас по умолчанию , и используйте директиву @tailwind, чтобы включить утилиты , заменяющие исходное содержимое файла:

```css
/* ./src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Устанавливаем официальные плавгины

```bash
npm install @tailwindcss/typography @tailwindcss/forms  @tailwindcss/line-clamp npm install @tailwindcss/aspect-ratio
```

И добавляем их вызывая в файле tailwind.config.js

```jsx
module.exports = {
  mode: "jit",
  purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/typography'),
    require('@tailwindcss/forms'),
    require('@tailwindcss/line-clamp'),
    require('@tailwindcss/aspect-ratio')
  ],
}
```
