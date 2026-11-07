# Safety policy

Только synthetic training; никаких production workloads.

Запрещены:
- secrets, PAT, OAuth client secrets, passwords, private keys, реальные secret values в файлах, logs или commits;
- GitHub Actions и каталог `.github/`;
- активные Woodpecker configurations без отдельного owner approval; сейчас нет `.woodpecker/` и root YAML;
- реальные deploy/release/publish steps, package/registry pushes, SSH, VM access, Docker build/push, privileged mode, Docker socket, host mounts;
- изменения firewall/DNS, private/production repositories, аккаунт `tophqru`, глобальные подключения;
- network writes, pushes, webhook activation, OAuth submission/authorization, платежи и расписания в рамках этой подготовки.

Пример с `image: alpine` — только текст, контейнеры сейчас не запускаются. Будущий container execution и image pull требуют согласования. Не настраивать custom CI config path на `.txt`: расширение само по себе не является security boundary при нестандартной конфигурации сервера.

Нет `from_secret`, services, plugins, external uploads или deploy commands. Missing-secret case использует только отсутствующий dummy input. Не печатать environment; не превращать отсутствие secret в success. При неожиданном доступе или output остановиться и зафиксировать BLOCKED без раскрытия данных.
