# Шаблон для агентной разработки

Стартовый шаблон проекта для работы с AI-агентами: Claude Code, Codex и OpenCode. Даёт общие инструкции для агентов, spec-driven процесс на [OpenSpec](https://github.com/Fission-AI/OpenSpec), набор MCP-серверов и скиллы для UI.

## Что внутри

| Файл | Назначение |
|---|---|
| `AGENTS.md` | Общие инструкции для всех агентов |
| `CLAUDE.md` | Подключает `AGENTS.md` для Claude Code |
| `BOOTSTRAP.md` | Одноразовая инструкция для агента: развернуть шаблон под проект |
| `.mcp.json` | MCP-серверы для Claude Code |
| `.codex/config.toml` | MCP-серверы для Codex |
| `opencode.json` | MCP-серверы и разрешения для OpenCode |
| `.claude/settings.json` | Общие разрешения для Claude Code |
| `.env.example` | Переменные окружения (строка подключения к БД) |
| `openspec/` | Спецификации и изменения OpenSpec |

MCP-серверы: `chrome-devtools` (браузер), `context7` (документация библиотек), `shadcn` (UI-компоненты), `postgres` (БД, только чтение).

Скиллы и команды OpenSpec в репозиторий не входят: их ставит агент по `BOOTSTRAP.md` свежими версиями в `.agents/skills/`.

## Требования

- Node.js (`npx`)
- Python 3 — для скилла UI/UX
- [uv](https://docs.astral.sh/uv/) (`uvx`) — для MCP-сервера `postgres`
- [OpenSpec CLI](https://github.com/Fission-AI/OpenSpec): `npm install -g @fission-ai/openspec`
- Хотя бы один агент: Claude Code, Codex или OpenCode

## Как начать

1. Создайте репозиторий из шаблона (кнопка «Use this template» на GitHub) или склонируйте его.
2. Откройте агента в корне проекта и напишите: «Выполни `BOOTSTRAP.md`».
3. Агент спросит про проект, стек и язык, установит OpenSpec и скиллы, подставит данные в `AGENTS.md`. Некоторые шаги придётся сделать руками, агент о них скажет:
   - подтвердить MCP-серверы в Claude Code (`/mcp`);
   - отметить проект как доверенный в Codex, иначе он не прочитает `.codex/config.toml`;
   - заполнить `.env` и экспортировать переменные в shell перед запуском агента, если нужна БД.
4. Начните первую фичу: `/opsx:propose "идея"` в Claude Code, `$openspec-propose "идея"` в Codex или `/opsx-propose "идея"` в OpenCode.

## Поддержка шаблона

- Меняя набор MCP-серверов или их версии, правьте все три конфига: `.mcp.json`, `.codex/config.toml`, `opencode.json`.
- Версии пакетов зафиксированы ради воспроизводимости. Обновляйте их явно.
- На Windows симлинк `.claude/skills` требует режима разработчика; шаблон рассчитан на macOS и Linux.

## Лицензия

[MIT](LICENSE)
