---
name: nc777-prompts
description: "NC 777 v3.0 — операционная система ролей с контрактами, роутером задач, приоритетами промптов и recovery-путями"
---

# NC 777 v3.0 — Role Operating System

Двойное ревью: DeepSeek + Codex. Вердикт: APPROVED.

---

## Быстрый старт / Роутер задач

| Тип задачи | Роли (минимальный трек) | Пример |
|---|---|---|
| 🟢 Мелкий баг | Pre-flight → Developer → QA → Cleaner | `пофиксить опечатку` |
| 🟡 Обычная фича | Prophet → Architect → Developer → QA → Cleaner | `добавить кнопку экспорта` |
| 🔴 Крупная фича | Prophet → Architect → Reviewer → Developer → QA → Saboteur → Cleaner | `система крепежа` |
| 🐛 Падающий тест/билд | Pitbull → Developer → QA → Documenter | `тест упал после merge` |
| 🔄 Рефакторинг | Architect → Developer → QA → Saboteur → Cleaner | `вынести общий код` |
| 🎨 UI-правка | Pre-flight → Developer → QA | `поправить верстку` |
| 🧹 Чистка | Cleaner (solo) | `удалить мёртвый код` |

**Правило:** если не уверен в сложности — бери трек выше.

---

## Конвейер (9 ролей — Prophet + Pre-flight объединены)

```
Prophet/Intake → Architect → Reviewer → Developer → Documenter → QA → Saboteur → Pitbull → Cleaner
```

---

## 1. Prophet / Intake (входной контроль)

**Use when:** любая задача сложнее опечатки

| Поле | Значение |
|---|---|
| Inputs | Задача от пользователя, рабочая директория |
| Must do | `node -c`, `curl` сервера, `git status`, список файлов |
| Must NOT do | Гадать без проверки, читать >5 файлов без плана |
| Output | Risk map: 3 вероятные точки отказа |
| Done when | Pre-flight зелёный + риски задокументированы |
| Stop when | Сервер не отвечает, битый JS, dirty worktree |

### Required
```
list the error states, empty states, and edge cases for {feature}
```
```
which files would I need to touch to {change}?
```

### Conditional
```
what would break if I deleted {target}?
```
```
here is a build error. fix the root cause and verify the build succeeds
```

---

## 2. Architect (план)

**Use when:** >3 файлов или >1 час работы

| Поле | Значение |
|---|---|
| Inputs | Risk map от Prophet, кодовая база |
| Must do | Аудит исходников, не предположения. План: файлы + строки + код + тесты |
| Must NOT do | Писать код, коммитить, обобщать «как-нибудь» |
| Output | SPEC.md или Markdown план с номерами строк |
| Done when | Reviewer approved или план покрывает все входы/выходы |
| Stop when | >5 файлов и неясна цель — уточнить |

### Required
```
plan how to refactor the {target} to {goal}. list files, don't edit yet
```
```
I want to build {feature}. interview me until covered, then write spec
```

### Conditional
```
look at how {example} is implemented, then build {new} the same way
```

---

## 3. Reviewer (критический взгляд)

**Use when:** крупная фича или план трогает >3 файла

| Поле | Значение |
|---|---|
| Inputs | План от Architect, git diff (если есть код) |
| Must do | Фальсифицировать: что сломается? Нестыковки? Пропущенные файлы? |
| Must NOT do | Писать код, соглашаться не глядя |
| Output | Review Report: VERDICT + risks |
| Done when | Все блокеры помечены |
| Stop when | CRITICAL → возврат к Architect |

### Required
```
review my uncommitted changes and flag anything risky
```
```
review PR #{pr} and summarize what changed, list concerns
```

### Conditional
```
use a subagent to review {path} for security issues
```
```
find every place we say "{copy}", show context, update to "{new}"
```

### Optional: Diff Review
```
show complete diff, highlight every change that could cause regression
```
### Optional: Dependency Audit
```
audit {package.json|requirements.txt} for outdated/vulnerable deps
```

---

## 4. Developer (реализация)

**Use when:** план утверждён

| Поле | Значение |
|---|---|
| Inputs | План + Review Report |
| Must do | Минимальные правки. Один файл = один коммит. Следовать паттернам |
| Must NOT do | «Заодно поправлю», рефакторинг вне плана |
| Output | Работающий код + тесты проходят |
| Done when | `node -c` OK + curl 200 + тесты зелёные |
| Stop when | Правка ломает тесты → Pitbull |

### Required
```
read issue #{issue}, implement the fix, and run the tests
```
```
look at how {example} is implemented, build {new} same way
```

### Conditional
```
explain what {path} does and how data flows through it
```
```
migrate everything from {from} to {to}: identify every place, make changes
```

---

## 5. Documenter (без напоминаний)

**Use when:** задача завершена или найдено новое правило

| Поле | Значение |
|---|---|
| Inputs | Итоги сессии, git diff |
| Must do | git diff → commit message → ?v= update |
| Must NOT do | Пропускать ?v=, коммитить без сообщения |
| Output | Commit + обновлённый кеш-бастер |
| Done when | Коммит сделан, ?v= обновлён |
| Stop when | Конфликт при коммите |

### Required
```
summarize what we did this session, suggest what to add to CLAUDE.md
```
### Conditional
```
create a /{name} skill for this project that {steps}
```
### Optional: Commit Message
```
write a conventional commit message for these changes
```

---

## 6. QA (верификация)

**Use when:** Developer завершил

| Поле | Значение |
|---|---|
| Inputs | Код от Developer, план тестирования |
| Must do | Все сценарии, без исключений. Playwright + grep + curl |
| Must NOT do | «Вроде работает», только happy path |
| Output | QA Report: pass/fail + скриншоты |
| Done when | Все тесты pass, нет регрессий |
| Stop when | FAIL → Pitbull |

### Required
```
write tests for {path}, run them, and fix any failures
```
```
the {test} test is failing, find out why and fix it
```

### Conditional
```
write tests for {feature} first, then implement until they pass
```
```
read {report}, add tests for lowest-covered files until above {target}%
```

### Optional: Accessibility
```
check {page|component} for accessibility: contrast, labels, keyboard nav
```

---

## 7. Saboteur (проактивный взлом)

**Use when:** крупная фича или рефакторинг

| Поле | Значение |
|---|---|
| Inputs | Готовый код после QA |
| Must do | Активно ломать: null, undefined, двойные клики, крайние случаи |
| Must NOT do | Повторять QA-тесты, патчить найденные баги |
| Output | Sabotage Report: что сломано + как воспроизвести |
| Done when | Все edge cases пройдены, ничего не упало |
| Stop when | Найден баг → Developer |

**vs Pitbull:** Saboteur атакует РАБОТАЮЩИЙ код. Pitbull чинит УПАВШЕЕ.

### Required
```
list error states, empty states, edge cases for {feature}
```
```
users are seeing {symptom} on {where}. investigate
```

### Conditional
```
what would break if I deleted {target}?
```

---

## 8. Pitbull (реактивное восстановление)

**Use when:** тест/билд/продакшен упал

| Поле | Значение |
|---|---|
| Inputs | Упавший тест/лог ошибки |
| Must do | 3 попытки. Каждая — новый подход. Root cause, не симптомы |
| Must NOT do | Патчить симптомы, сдаваться после 1 попытки |
| Output | Pitbull Attempt Log |
| Done when | Тест зелёный, причина устранена |
| Stop when | 3 попытки → честный рапорт |

### Required
```
the {test} test is failing, find out why and fix it
```
```
here is a build error. fix the root cause and verify build succeeds
```

### Conditional
```
{symptom}. check logs, recent deploys, config changes. most likely cause?
```

### Optional: Performance
```
profile {endpoint|function} and bring {metric} under {target}
```

---

## 9. Cleaner (порядок после боя)

**Use when:** задача завершена, все тесты зелёные

| Поле | Значение |
|---|---|
| Inputs | Финальное состояние кода |
| Must do | Удалить временные файлы, скрипты-однодневки, закомментированный код |
| Must NOT do | Удалять продакшен-код, ломать работающее |
| Output | Cleaner Summary: что удалено + git status |
| Done when | Worktree чистый |
| Stop when | Неясно, нужен ли файл → спросить |

⚠️ Destructive prompts — только после зелёных тестов.

### Required
```
that is too much. keep only changes to {scope}, undo other edits
```
### Conditional
```
you keep {mistake}. add a rule to CLAUDE.md so this stops happening
```

---

## Steering — для любой роли

```
that is not right: {feedback}. try a different approach
```

---

## Шаблоны вывода

### Architect Plan
```markdown
## Goal: [1 sentence]
## Files: [list with line numbers]
## Steps: [numbered]
## Risks: [top 3]
## Verification: [exact commands]
```

### Review Report
```markdown
## Verdict: APPROVED | NEEDS CHANGES
## Critical: [blockers]
## Warnings: [non-blockers]
## Missed: [files/scenarios missed]
```

### QA Report
```markdown
## Results: PASS | FAIL
## Scenarios: [N/M passed]
## Evidence: [screenshots, curl, grep]
## Regressions: [if any]
```

### Pitbull Attempt Log
```markdown
## Attempt 1: [approach] → [result]
## Attempt 2: [approach] → [result]  
## Attempt 3: [approach] → [result]
## Verdict: FIXED | REPORTED
```

### Cleaner Summary
```markdown
## Removed: [files]
## Kept: [why]
## git status: [clean]
```

---

## Recovery Matrix

| State | Next action | Escalate when |
|---|---|---|
| Unclear requirements | Architect interview prompt → clarify tool | >2 цикла |
| Too many files touched | Cleaner narrow scope | >5 unexpected files |
| Test failing (3 attempts) | Pitbull honest report | — |
| Destructive operation | Ask user confirmation | No backup |
| Tool missing | Alternative (curl→wget, Playwright→grep) | No alternative |
| Bug not reproducible | Saboteur edge case → document | >3 attempts |

---

## Адаптация под Hermes

- `subagent` → `delegate_task`
- `CLAUDE.md` → `skill_manage` или `memory`
- `/name` skill → `skill_manage(action='create')`
- Pre-flight: `node -c app.js` + `curl localhost` + `git status`
- QA: Playwright
