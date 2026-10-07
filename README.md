# Claude Skills для недвижимости: MCP-сервер, CRM и объекты для ИИ-агента риелтора

[Русский](#русский) · [English](#english)

## Русский

Набор **Claude skills** и плагин для **Claude Code**, которые подключают ИИ-агента к вашему сайту и CRM на платформе
[Sitebill](https://www.sitebill.ru/mcp-server/) через **MCP-сервер**. Агент ищет объекты недвижимости, делает подборки
для клиентов, ведёт контакты и сделки, создаёт черновики объявлений, а вы просто формулируете задачу словами.

**Страница продукта и подробное описание: https://www.sitebill.ru/mcp-server/**

### Что умеет агент

| Задача | Инструменты MCP |
|---|---|
| Поиск объектов и подборка для клиента | `get_search_options`, `resolve_location`, `search_properties`, `get_property`, `create_selection` |
| Контакты клиентов | `list_contacts`, `get_contact`, `create_contact`, `add_contact_note` |
| Сделки в воронке | `list_deals`, `get_deal`, `create_deal`, `move_deal`, `add_deal_comment` |
| Объекты в базе | `create_property` (по умолчанию черновик), `update_property` |

Всего 16 инструментов. Полный справочник параметров: [tools.md](plugins/sitebill-realty/skills/sitebill-mcp/references/tools.md).
Удаления объектов, контактов и сделок в MCP-сервере нет.

### Примеры запросов к агенту

- «Подбери клиентке Ивановой двушку до 9 млн в центре и сделай ссылку на подборку».
- «Заведи контакт: Пётр, телефон +7…, ищет студию в аренду, и создай сделку».
- «Что у меня висит в сделках на стадии "Показ"? Кому давно не писал?»
- «Добавь заметку к сделке: клиент ждёт ответа банка».
- «Создай черновик объекта по этому описанию».

Больше сценариев: [workflows.md](plugins/sitebill-realty/skills/sitebill-mcp/references/workflows.md).

### Быстрый старт

1. **Нужен сайт на платформе.** Создайте бесплатный демо-сайт «Риелтор Старт»:
   [создать демо](https://www.sitebill.ru/client/cart.php?a=add&pid=126).
2. **Включите MCP.** В админке сайта: «Настройки → ИИ-агенты». Затем в кабинете, раздел «ИИ-агенты», выпустите токен
   (он показывается один раз). Токен сотрудника работает с его правами, ключ организации только для чтения и без CRM.
3. **Подключите агента.**

**Claude Code, как плагин** (скилл и сервер сразу):

```bash
/plugin marketplace add rumantic/claude-skills-real-estate
/plugin install sitebill-realty@claude-skills-real-estate
```

Плагин читает адрес и токен из переменных окружения:

```bash
export SITEBILL_MCP_URL="https://ВАШ-САЙТ/mcp"
export SITEBILL_MCP_TOKEN="ВАШ_ТОКЕН"
```

**Claude Code, только сервер:**

```bash
claude mcp add --transport http realty https://ВАШ-САЙТ/mcp --header "Authorization: Bearer ВАШ_ТОКЕН"
```

**Скилл вручную:** скопируйте папку `plugins/sitebill-realty/skills/sitebill-mcp` в `~/.claude/skills/sitebill-mcp`
(или в `.claude/skills/` проекта). Он научит агента правильному порядку работы с инструментами и правилам безопасности.

Для **Claude Desktop** и **Cursor** инструкция в [docs/connect.md](docs/connect.md).

### Безопасность

- Агент работает с правами сотрудника, на которого выпущен токен, и не видит больше, чем вы сами.
- Права `mcp:read` и `mcp:write` разделены: токен только для чтения ничего не изменит.
- Новые объекты создаются черновиками и не попадают на сайт, пока вы их не опубликуете.
- Тексты объявлений и заметок написаны третьими лицами. Агент считает их данными, а не инструкциями.
- Каждый вызов пишется в журнал, частота запросов ограничена, токен отзывается одним кликом.
- Сервер выключен по умолчанию, пока вы не включили его в настройках.

### Для ИИ-агентов

Если вы агент или разрабатываете агентов: описание сервера простым текстом лежит на
https://www.sitebill.ru/mcp-server.md, указатель сайта: https://www.sitebill.ru/llms.txt.
Транспорт: Streamable HTTP, JSON-RPC 2.0, авторизация `Authorization: Bearer <токен>`.
Правила работы с инструментами для агента: [SKILL.md](plugins/sitebill-realty/skills/sitebill-mcp/SKILL.md).

### Ключевые слова

claude skills, claude code skills, agent skills, mcp сервер, mcp server, ИИ-агент для риелтора, ИИ-агент для CRM,
ИИ агенты в недвижимости, CRM для риелторов, подключить ИИ к CRM, real estate, realtor, Sitebill.

## English

**Claude skills and a Claude Code plugin for real estate agents.** They connect an AI agent (Claude, Cursor and other
MCP clients) to your listings, contacts and deals on the [Sitebill](https://www.sitebill.ru/mcp-server/) platform through
its **MCP server**: search properties, build client selections, manage CRM contacts and deals, create draft listings.

- Product page (Russian): https://www.sitebill.ru/mcp-server/
- Machine-readable description: https://www.sitebill.ru/mcp-server.md
- Transport: Streamable HTTP, JSON-RPC 2.0, `Authorization: Bearer <token>`; endpoint `https://<your-site>/mcp`
- 16 tools: search and selections, contacts, deals, listings (create as draft, update). No delete operations.

```bash
/plugin marketplace add rumantic/claude-skills-real-estate
/plugin install sitebill-realty@claude-skills-real-estate
```

## Лицензия

[MIT](LICENSE). Исходный код платформы в этот репозиторий не входит.
