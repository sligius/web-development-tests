# Коллекция тестов на внимание и когнитивные способности

Этот проект представляет собой веб-приложение, содержащее набор тестов, предназначенных для оценки различных аспектов внимания и когнитивных способностей. Тесты разработаны с использованием Vue.js и включают в себя:

## Список тестов:

1. **Тест Мюнстерберга:** Классический тест на внимание, требующий поиска скрытых слов в тексте за ограниченное время. Оценивает способность к концентрации и избирательной обработке информации.

2. **Фланговый тест Эриксена:** Тест на концентрацию и скорость обработки информации. Необходимо быстро реагировать на центральную стрелку, игнорируя отвлекающие стрелки вокруг неё.

3. **Тест на пространственное мышление:** Состоит из нескольких вопросов, требующих визуального представления и пространственного мышления. Оценивает способность ориентироваться в пространстве и мыслить образами.

4. **Сравнение чисел:** Тест на внимание к деталям, где нужно сравнить пары чисел и определить их равенство или различие за ограниченное время.

## Структура проекта:

Проект организован с использованием Vue.js компонентной архитектуры. Каждый тест реализован как отдельный компонент Vue (.vue файл). Главный компонент управляет переключением между тестами.

## Установка и запуск проекта Vue.js без сборки (build)

### Предварительные условия:

Убедитесь, что у вас установлены Node.js и npm (или yarn).

### Шаги:

1. **Клонирование репозитория (или скачивание проекта):**

   Скачайте исходный код проекта Vue.js. Если проект хранится на GitHub или GitLab, клонируйте его с помощью Git:

   `git clone <ссылка*на*репозиторий>`

   `cd <имя*папки*проекта>`

   Или скачайте его как zip-архив и распакуйте в нужное место.

2. **Установка зависимостей:**

   Перейдите в директорию проекта в терминале и установите необходимые пакеты:

   `npm install`

   или

   `yarn install`

3. **Запуск сервера разработки:**

   После установки зависимостей запустите сервер разработки. Команда обычно находится в файле package.json в секции scripts. Возможные варианты:

   `npm run serve` или `npm run dev`

   или

   `yarn serve` или `yarn dev`

4. **Просмотр приложения:**

   После запуска сервера, откройте браузер и перейдите по адресу, указанному в терминале (обычно http://localhost:5173 или подобный).

## Использование:

Приложение предоставляет простой интерфейс для выбора и прохождения тестов. После завершения каждого теста отображаются результаты с подробным анализом и интерпретацией. Есть возможность начать тест заново.
