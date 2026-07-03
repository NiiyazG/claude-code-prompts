# Полезные промты из Claude Code Prompt Library

Сохранено 03.07.2026 из https://code.claude.com/docs/en/prompt-library

## 🔍 Понимание кодовой базы

### Обзор проекта
```
give me an overview of this codebase: architecture, key directories, and how the pieces connect
```

### Объяснить незнакомый код
```
explain what {path} does and how data flows through it. write it up as {format}
```
Пример: `src/scheduler/queue.ts` → `an HTML page with a diagram`

### Найти где что происходит (поиск по поведению)
```
where do we {behavior}?
```
Пример: `validate uploaded file types`

### Что сломается если удалить
```
what would break if I deleted {target}?
```
Пример: `the retryWithBackoff helper`

### История изменений файла
```
look through the commit history of {path} and summarize how it evolved and why
```

### Пройти по коду как PM
```
I am a {role}. walk me through what happens when a user {action}, from the UI down to the result
```
Пример: `PM` → `clicks Export to PDF`

## 🏗️ Планирование и дизайн

### Оценить масштаб изменений до начала
```
which files would I need to touch to {change}?
```
Пример: `add a dark mode toggle to settings`

### План рефакторинга (без правок)
```
plan how to refactor the {target} to {goal}. list the files you would change, but don't edit anything yet
```
Пример: `payment module` → `support multiple currencies`

### Спецификация через интервью
```
I want to build {feature}. interview me about implementation, UX, edge cases, and tradeoffs until we have covered everything, then write the spec to SPEC.md
```

### Карта граничных случаев
```
list the error states, empty states, and edge cases for {feature} that the design needs to cover
```
Пример: `the file upload flow`

## 💻 Реализация

### Следовать существующему паттерну
```
look at how {example} is implemented to understand the pattern, then build {new} the same way
```
Пример: `the GitHub webhook handler` → `a Stripe webhook handler`

### Сгенерировать документацию для кода
```
find {scope} without {format} comments and add them, matching the style already used in the file
```
Пример: `the public functions in src/auth/` → `JSDoc`

### Маленькая фича
```
add a {endpoint} endpoint that returns {payload}
```
Пример: `/health` → `the app version and uptime`

### Внутренний инструмент с нуля
```
create a {tool} using HTML, CSS, and vanilla JavaScript, then open it in my browser
```
Пример: `drag-and-drop Kanban board with three columns`

### Issue → код
```
read issue #{issue}, implement the fix, and run the tests
```

### Найти и заменить текст во всём проекте
```
find every place we say "{copy}" or a close variant, show me each one in context, then update them all to "{new}". leave tests and the changelog alone
```

### Документ по образцу
```
read the {examples} in {folder} to learn the structure and voice, then draft a new one for {topic}
```
Пример: `privacy impact assessments` → `legal/pia/` → `the new analytics integration`

## 🧪 Тестирование

### Написать тесты → запустить → исправить
```
write tests for {path}, run them, and fix any failures
```

### TDD: тесты → реализация
```
write tests for {feature} first, then implement it until they pass
```
Пример: `the password reset flow`

### Покрытие из отчёта
```
read {report} and add tests for the lowest-covered files until each is above {target}%
```
Пример: `coverage/coverage-summary.json` → `80`

## 🔧 Рефакторинг

### Миграция паттерна
```
migrate everything from {from} to {to}: identify every place that needs to change, then make the changes
```
Пример: `the old logging API` → `the structured logger`

### Порт между языками
```
port {source} to {target}, keeping the same {keep}
```
Пример: `this Python module` → `Rust` → `public API and test behavior`

### Оптимизация по метрике
```
optimize {target} to bring {metric} from {current} down to under {goal}
```
Пример: `the search query` → `p95 latency` → `2s` → `500ms`

### Визуальный баг
```
the {element} extends {amount} beyond the {container} on {viewport}. fix it.
```
Пример: `login button` → `20px` → `card border` → `mobile`

## 👀 Ревью

### Проверить свои изменения перед коммитом
```
review my uncommitted changes and flag anything that looks risky before I commit
```

### Ревью PR
```
review PR #{pr} and summarize what changed, then list any concerns
```

### Ревью инфраструктуры
```
here is my Terraform plan output. what is this going to do, and is anything here going to cause problems?
```

### Security review через subagent
```
use a subagent to review {path} for security issues and report what it finds
```

### Ревью контента
```
review {file} for {concerns} and list anything I should fix before it goes to {reviewer}
```
Пример: `launch-post.md` → `unsupported claims, missing attributions` → `legal`

## 🎯 Steering (корректировка)

### Исправить подход
```
that is not right: {feedback}. try a different approach
```
Пример: `the function signature needs to stay backward-compatible`

### Сузить скоуп
```
that is too much. keep only the changes to {scope} and undo your other edits
```
Пример: `the validation logic in src/forms/`

### Превратить исправление в правило
```
you keep {mistake}. add a rule to CLAUDE.md so this stops happening
```
Пример: `using default exports when this project uses named exports`

## 🐛 Отладка

### Упавший тест
```
the {test} test is failing, find out why and fix it
```

### Ошибка от пользователей
```
users are seeing {symptom} on {where}. investigate and tell me what is going on
```
Пример: `500 errors` → `/api/settings`

### Ошибка сборки
```
here is a build error. fix the root cause and verify the build succeeds
```

### Продакшен-инцидент
```
{symptom}. check the logs, recent deploys, and config changes, then tell me the most likely cause
```
Пример: `the checkout endpoint started returning 500s an hour ago`

### Логи запросом
```
show me all {events} for {scope} over {timeframe}. write the query, run it, and tell me what stands out
```
Пример: `failed logins` → `the auth service` → `the past 24 hours`

## 📊 Данные

### Анализ CSV
```
read {file}, summarize the key patterns, and write the results to {output}
```
Пример: `@reports/q1-signups.csv` → `an HTML page with charts, then open it in my browser`

### Генерация вариаций из данных
```
read {file}, find the underperforming {items}, and generate {n} new variations that stay under {limit} characters
```
Пример: `@ads-performance.csv` → `headlines` → `20` → `90`

## 🤖 Автоматизация

### Создать skill для повторяющейся задачи
```
create a /{name} skill for this project that {steps}
```
Пример: `ship` → `runs the linter and tests, then drafts a commit message`

### Хук после события
```
write a hook that {action} after every {event}
```
Пример: `runs prettier` → `edit to a .ts or .tsx file`

### Сохранить итоги сессии
```
summarize what we did this session and suggest what to add to CLAUDE.md
```
