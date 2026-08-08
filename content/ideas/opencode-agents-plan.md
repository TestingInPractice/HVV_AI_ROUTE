---
title: "План: вникание в агенты OpenCode"
description: План изучения агентов по гайду datatalks.ru/opencode + полный перечень ссылок.
tags:
  - opencode
  - агенты
  - план
---

# План: вникание в агенты OpenCode

База для плана: гайд [datatalks.ru/opencode](https://datatalks.ru/opencode/) (глава 00–09).

## Шаг 0 — контекст (~15 мин)

Прочитай главу **00 (index)** и **01 Быстрый старт**. Убедись, что `/init`, `/connect`, `AGENTS.md` и минимальный `opencode.json` на автомате.

- https://datatalks.ru/opencode/index.html — введение, карта гайда
- https://datatalks.ru/opencode/01-quick-start.html — установка, /connect, /init, первый AGENTS.md, чек-лист 30 минут

## Шаг 1 — анатомия (~30 мин)

Глава **02** на примере `ai-agent-codex`: связка «AGENTS.md задаёт смысл → opencode.json задаёт механику → specs хранят intent → agents делят работу → skills дают экспертизу». Сравни со своим CodeAIStage2, найди 3 отличия.

- https://datatalks.ru/opencode/02-project-anatomy.html

## Шаг 2 — агенты (ядро, 1–2 часа)

Глава **03** + документация по агентам. Критерий: субагент нужен, когда у роли есть **operational boundary**. Антипаттерн — «субагент ради названия». Разбери frontmatter субагента (`mode: subagent`, `temperature`, `steps`, `permission`) и разницу rules vs skills vs commands.

- https://datatalks.ru/opencode/03-agents-and-skills.html
- https://opencode.ai/docs/ru/agents/

## Шаг 3 — конфиг и permissions (~1 час)

Глава **04** + документация по конфигу. Паттерны: `instructions` как композиция, whitelist для `permission.skill`, `allow/ask/deny`, `permission.task` как routing guardrail. Проверь свой `.opencode/opencode.json`.

- https://datatalks.ru/opencode/04-config-and-permissions.html
- https://opencode.ai/docs/ru/config/

## Шаг 4 — командные процессы (~1 час)

Глава **05** и **07**: модель зрелости Stage 1–5, Plan→Spec→Build→Verify, антипаттерны, модельная стратегия и cost-контроль.

- https://datatalks.ru/opencode/05-team-workflows.html
- https://datatalks.ru/opencode/07-best-practices.html
- https://opencode.ai/docs/ru/tui/
- https://opencode.ai/pricing

## Шаг 5 — практика на себе (2–3 часа)

Глава **09** + проект azbuka: цикл plan → build → verify → review → stage report; написать нового субагента (`@review`, read-only) и skill (`/tdd` или `/postmortem`); использовать готовые промпты из главы 09.

- https://datatalks.ru/opencode/09-project-example.html
- https://datatalks.ru/opencode/09-project-example-step-2.html

## Шаг 6 — MCP и глубина (опционально, ~1 час)

Глава **06** + документация по MCP. MCP расходует контекст — включай серверы на уровне агента, а не глобально.

- https://datatalks.ru/opencode/06-mcp-and-integrations.html
- https://opencode.ai/docs/ru/mcp-servers/

## Шаг 7 — систематизация

Сохрани этот план как заметку в базу знаний CodeAI.

---

# Все ссылки гайда и что они значат

## Главы самого гайда (datatalks.ru/opencode/)

| Ссылка | Что внутри |
|---|---|
| https://datatalks.ru/opencode/index.html | Введение, карта гайда, рамка «OpenCode как слой проекта» |
| https://datatalks.ru/opencode/01-quick-start.html | Установка, /connect, /init, первый AGENTS.md и opencode.json, чек-лист 30 минут |
| https://datatalks.ru/opencode/02-project-anatomy.html | Разбор репо-примера: 5 слоёв (AGENTS.md, opencode.json, specs, agents, skills) |
| https://datatalks.ru/opencode/03-agents-and-skills.html | Главное про агентов: когда субагент оправдан, frontmatter, prompts, skills, rules vs skills vs commands |
| https://datatalks.ru/opencode/04-config-and-permissions.html | opencode.json как control plane, allow/ask/deny, whitelist skills, task routing |
| https://datatalks.ru/opencode/05-team-workflows.html | Plan→Spec→Build→Verify, маршрутизация по ролям, specs-first, модель зрелости Stage 1–5 |
| https://datatalks.ru/opencode/06-mcp-and-integrations.html | MCP локально/удалённо, OAuth, управление контекстом, включение MCP на уровне агента |
| https://datatalks.ru/opencode/07-best-practices.html | Модельная стратегия (Zen), Plan/Build дисциплина, skills /review /deploy /tdd /postmortem, cost-контроль |
| https://datatalks.ru/opencode/08-development-process.html | Delivery cycle (Plan→Implement→Verify→Review→Report), моно/мультиагенты, permissions как boundary, infra-guardrails |
| https://datatalks.ru/opencode/09-project-example.html | Готовый AGENTS.md-шаблон, Stage Report/Definition, промпты plan/build/review/debug, online shop пример |
| https://datatalks.ru/opencode/09-project-example-step-2.html | Структура /docs для shop-проекта: stage files, reports, ADR, runbooks |

## Официальная документация OpenCode (основа гайда)

| Ссылка | Что значит |
|---|---|
| https://opencode.ai/docs/ru/ | Корень официальной рус. документации (локальная копия в CodeAI `docs/opencode-docs/`) |
| https://opencode.ai/docs/ru/agents/ | Документация по агентам — первичный источник для главы 03 |
| https://opencode.ai/docs/ru/config/ | Конфиг opencode.json — для главы 04 |
| https://opencode.ai/docs/ru/tui/ | TUI-интерфейс (plan/build, режимы) — для главы 05 |
| https://opencode.ai/docs/ru/mcp-servers/ | MCP-серверы — для главы 06 |
| https://opencode.ai/pricing | Калькулятор стоимости моделей — контроль бюджета (глава 07) |
| https://opencode.ai/install | Установочный скрипт (глава 01) |

## Репозитории-эталоны

| Ссылка | Что значит |
|---|---|
| https://github.com/ivanshamaev/ai-agent-codex | Пример проекта, на котором построен весь гайд: 7 агентов + 8 skills + specs. Сравнивай со своим CodeAIStage2 |
| https://github.com/yusufkaraaslan/Skill_Seekers | Коллекция готовых OpenCode skills + кейсы, где их применять |
| https://github.com/nidhinjs/prompt-master | Шаблоны и tooling для поддержки skills/rules в актуальном состоянии |
| https://github.com/alirezarezvani/claude-skills/ | Claude-навыки, легко адаптируемые под OpenCode |
| https://github.com/phuryn/pm-skills | Skills для продуктовых задач (discovery, roadmap, требования) — для PM-ролей |
| https://github.com/VoltAgent/awesome-agent-skills | Консолидированный список агентных skills, включая OpenCode-шаблоны |

## Каталоги skills и MCP

| Ссылка | Что значит |
|---|---|
| https://cursor.directory/ | Каталог skills и MCP с фильтрами по назначению и языкам |
| https://mcpmarket.com/ | Маркетплейс MCP-серверов (документация, DevOps, корпоративные API) |
| https://skills.sh/ | Онлайн-редактор с превью, тегами, примерами промптов |
| https://skillhub.club | Сообщество с подборками skills и инструкциями адаптации |
| https://skillsmp.com | Маркетплейс кастомных skills/rules с ценами поддержки |

## Обучение и видео

| Ссылка | Что значит |
|---|---|
| https://fkonovalov.github.io/claude-skills-guide-ru/ | Русскоязычный гайд по проектированию skills: структура, версионирование, адаптация |
| https://www.youtube.com/watch?v=KSEfr8h-tH8 | Видеообзор интерфейса OpenCode: TUI/десктоп, провайдеры, подписки Claude/GPT-Codex |
| https://www.youtube.com/watch?v=z7901BIReGs | Расширенный туториал: установка, MCP, skills, rules, генерация промптов |

## MCP-сервисы из главы 06

| Ссылка | Что значит |
|---|---|
| https://mcp.context7.com/mcp | Context7 — актуальная документация библиотек (подключён в CodeAIStage2) |
| Grep by Vercel / grep.app | Поиск кода по открытым репозиториям (сценарный MCP из главы 06) |

---

## Связанные идеи
- [[example-idea|Пример идеи]]
- [[example-step|Пример шага]]
