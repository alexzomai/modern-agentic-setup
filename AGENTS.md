# AGENTS.md

Инструкции для AI-агентов, работающих в проекте `<project-name>`.

> Если проект только создан из шаблона, сначала выполните `BOOTSTRAP.md`.

## Процесс разработки: OpenSpec

Изменения ведутся через spec-driven workflow [OpenSpec](https://github.com/Fission-AI/OpenSpec).

- `openspec/specs/` — текущие спецификации (источник истины о поведении системы).
- `openspec/changes/` — активные предложения изменений (proposal, design, tasks, delta-спеки).
- `openspec/config.yaml` — конфигурация OpenSpec.

Порядок работы:

1. Для новой функциональности или существенного изменения сначала создайте change (скилл `openspec-propose`).
2. Реализуйте задачи из `tasks.md` изменения (скилл `openspec-apply-change`), отмечая выполненные.
3. После завершения архивируйте change (скилл `openspec-archive-change`), чтобы delta-спеки влились в `openspec/specs/`.

Мелкие правки (опечатки, форматирование, очевидные баги) можно делать без change.

Как вызвать workflow в разных агентах:

| Действие | Claude Code | Codex | OpenCode |
|---|---|---|---|
| Предложить change | `/opsx:propose` | `$openspec-propose` | `/opsx-propose` |
| Реализовать | `/opsx:apply` | `$openspec-apply-change` | `/opsx-apply` |
| Архивировать | `/opsx:archive` | `$openspec-archive-change` | `/opsx-archive` |
| Исследовать | `/opsx:explore` | `$openspec-explore` | `/opsx-explore` |
| Обновить план change | `/opsx:update` | `$openspec-update-change` | `/opsx-update` |
| Влить delta-спеки без архивации | `/opsx:sync` | `$openspec-sync-specs` | `/opsx-sync` |

## Правила для агентов

- Перед изменением поведения прочитайте релевантные спеки в `openspec/specs/`.
- Не расходитесь со спецификацией молча: если код и спека конфликтуют, обновите спеку через change.
- Делайте минимальные, сфокусированные изменения; стиль нового кода должен совпадать с окружающим.
- Не коммитьте секреты и `.env`-файлы.
- Не выполняйте коммиты и пуши без явной просьбы пользователя.

## MCP

Серверы описаны в трёх файлах, по одному на агента: `.mcp.json` (Claude Code), `.codex/config.toml` (Codex), `opencode.json` (OpenCode). Меняя набор серверов или версии, правьте все три.

- `chrome-devtools` — открытие страниц и проверка UI (`new_page`, `navigate_page`, `take_snapshot`, `take_screenshot`). Открывает отдельный экземпляр Chrome со своим профилем, а не рабочий браузер пользователя.
- `context7` — актуальная документация библиотек.
- `shadcn` — поиск и добавление компонентов shadcn/ui.
- `postgres` — схема БД и запросы в режиме read-only (`--access-mode=restricted`). Строка подключения берётся из `DATABASE_URI` (см. `.env.example`). Не записывайте пароли в конфиги MCP. Если БД не используется, удалите сервер из всех трёх файлов.

## Скиллы

Скиллы лежат в `.agents/skills/*/SKILL.md` и ставятся по `BOOTSTRAP.md`. Codex и OpenCode читают `.agents/skills/` напрямую; для Claude Code `.claude/skills` — симлинк на неё. Агентам без поддержки скиллов читать и применять их нужно вручную.

- Для работы с интерфейсами сначала прочитайте `.agents/skills/ui-ux-pro-max/SKILL.md` (если скилл установлен).
- Поиск по данным скилла (нужен Python 3):
  `python3 .agents/skills/ui-ux-pro-max/scripts/search.py "<запрос>" --design-system -p "<project-name>"`

## Команды

<!-- Заполнить после выбора стека -->

- Установка зависимостей: `<команда>`
- Сборка: `<команда>`
- Тесты: `<команда>`
- Линтер: `<команда>`
- Запуск: `<команда>`
