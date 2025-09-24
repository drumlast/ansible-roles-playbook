# Ansible Roles: ClickHouse & Vector

Этот репозиторий содержит роли Ansible для автоматизированного развёртывания **ClickHouse** и **Vector**.

## Роль: `clickhouse`

### Описание
Роль устанавливает и настраивает ClickHouse из официального репозитория.

### Переменные

| Переменная | Описание | Значение по умолчанию |
|------------|-----------|-----------------------|
| `clickhouse_version` | Версия ClickHouse (`latest` или фиксированная) | `latest` |
| `clickhouse_service_enable` | Включение автозапуска сервиса | `true` |
| `clickhouse_service_ensure` | Убедиться, что сервис запущен | `started` |
| `clickhouse_listen_host` | Список интерфейсов для прослушивания | `[::1, 127.0.0.1]` |
| `clickhouse_http_port` | HTTP порт | `8123` |
| `clickhouse_tcp_port` | TCP порт | `9000` |
| `clickhouse_users_default` | Пользователи ClickHouse | см. `defaults/main.yml` |

### Пример использования
```yaml
- hosts: clickhouse
  roles:
    - role: clickhouse
      vars:
        clickhouse_version: "latest"
```

---

## Роль: `vector_role`

### Описание
Роль устанавливает [Vector](https://vector.dev) — инструмент для сбора и передачи логов.

### Переменные

| Переменная | Описание | Значение по умолчанию |
|------------|-----------|-----------------------|
| `vector_repo_url` | URL для установки репозитория | `https://setup.vector.dev` |
| `vector_config_path` | Путь к конфигурационному файлу | `/etc/vector/vector.toml` |
| `vector_user` | Пользователь, от которого запускается сервис | `vector` |
| `vector_group` | Группа, от которой запускается сервис | `vector` |

### Пример использования
```yaml
- hosts: vector
  roles:
    - role: vector_role
      vars:
        vector_config_path: "/etc/vector/vector.toml"
```

---

## Роль: `lighthouse_role`

### Описание
Роль устанавливает [Lighthouse](https://github.com/VKCOM/lighthouse) — веб-интерфейс для ClickHouse.

### Переменные

| Переменная | Описание | Значение по умолчанию |
|------------|-----------|-----------------------|
| `lighthouse_git_repo` | Репозиторий Lighthouse | `https://github.com/VKCOM/lighthouse.git` |
| `lighthouse_base_dir` | Путь установки | `/var/www/lighthouse` |
| `lighthouse_nginx_site_path` | Путь к конфигу nginx | `/etc/nginx/sites-available/lighthouse.conf` |
| `lighthouse_nginx_site_link` | Ссылка на конфиг nginx | `/etc/nginx/sites-enabled/lighthouse.conf` |
| `lighthouse_listen_port` | Порт для Lighthouse | `8080` |
| `lighthouse_server_name` | Имя сервера nginx | `_` |
| `lighthouse_clickhouse_host` | Хост ClickHouse | `127.0.0.1` |
| `lighthouse_clickhouse_http_port` | HTTP порт ClickHouse | `8123` |

### Пример использования
```yaml
- hosts: lighthouse
  roles:
    - role: lighthouse_role
      vars:
        lighthouse_clickhouse_host: "192.168.245.10"
        lighthouse_clickhouse_http_port: 8123
```

---

## Лицензия
MIT