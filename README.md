# Vue Posts App

Одностраничное приложение для отображения и фильтрации публикаций с использованием Vue.js и Bootstrap 4.

## Описание

Приложение получает данные из [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) и предоставляет следующий функционал:

- Отображение списка публикаций в виде карточек
- Фильтрация публикаций по автору
- Поиск по заголовкам публикаций
- Счетчик найденных публикаций
- Модальное окно с полным текстом публикации
- Адаптивный дизайн для всех устройств

## Основные особенности

1. **Список публикаций:**
   - Карточки с заголовком и кратким содержанием
   - Анимация при наведении
   - Отображение автора публикации
   - Адаптивная сетка (2 колонки на десктопе, 1 на мобильных)

2. **Фильтрация и поиск:**
   - Выпадающий список авторов
   - Поиск по заголовкам в реальном времени
   - Динамический счетчик результатов
   - Комбинирование фильтров

3. **Модальное окно:**
   - Современные анимации появления
   - Эффект размытия фона
   - Полный текст публикации
   - Информация об авторе
   - Закрытие по клику вне окна или кнопке

4. **Технические особенности:**
   - TypeScript для типизации
   - SCSS для стилей
   - Компонентный подход
   - Адаптивный дизайн
   - Оптимизированные анимации

## Установка и запуск

```bash
# Установка зависимостей
npm install

# Запуск сервера разработки
npm run dev

# Сборка для продакшена
npm run build
```

## Использованные технологии

- Vue.js 3
- TypeScript
- Bootstrap 4
- SCSS
- Axios
- JSONPlaceholder API

## Структура проекта

- `src/components/PostsList.vue` - Основной компонент со списком публикаций
- `src/components/PostModal.vue` - Компонент модального окна
- `src/assets/` - Стили и ресурсы
- `src/App.vue` - Корневой компонент приложения

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Type Support for `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) to make the TypeScript language service aware of `.vue` types.

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Run End-to-End Tests with [Playwright](https://playwright.dev)

```sh
# Install browsers for the first run
npx playwright install

# When testing on CI, must build the project first
npm run build

# Runs the end-to-end tests
npm run test:e2e
# Runs the tests only on Chromium
npm run test:e2e -- --project=chromium
# Runs the tests of a specific file
npm run test:e2e -- tests/example.spec.ts
# Runs the tests in debug mode
npm run test:e2e -- --debug
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
