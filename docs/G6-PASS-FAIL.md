# G6: будущая test matrix

Статус всех реальных CI cases: **NOT RUN**. План не является evidence выполнения.

| Case | Изолированный будущий stimulus | Expected outcome | Evidence дополнительно |
| --- | --- | --- | --- |
| pass-case | `sh testcases/pass/check.sh` в отдельном workflow | exit 0; step/pipeline success; GitHub status success на том же SHA | `CI157_PASS_CASE` |
| fail-case | `sh testcases/fail/check.sh` в отдельном workflow | exit 1; step/pipeline failure; GitHub status failure на том же SHA, без false green | `CI157_FAIL_CASE`, exit code |
| timeout-ish | Только после одобрения: bounded synthetic wait дольше согласованного короткого runner timeout; внешний hard cap | Не success; timeout/cancel/failure согласно проверенной версии, процесс остановлен в установленный срок | согласованные limits, фактическая длительность, причина завершения |
| missing-secret-should-fail-safely | Только после одобрения: проверка отсутствующей dummy variable без secret injection, например `test -n "${CI157_DUMMY_REQUIRED:-}"` | ненулевой exit, failure до любых side effects; никаких значений env в logs | marker отсутствия dummy input, exit code, redacted excerpt |

Timeout и missing-secret cases пока только спецификация: не создавать реальный secret, не читать существующий, не печатать env; не запускать бесконечные циклы. Dummy input — не credential. Сначала согласовать конкретную реализацию, лимиты и поддерживаемые статусы установленной версии.

## Evidence для каждого реального case

- repository URL, branch, event и полный commit SHA;
- реальный pipeline URL на https://ci.top-hq.ru, pipeline/workflow/step ID;
- Woodpecker status и соответствующий GitHub commit status/check с тем же SHA;
- exit code, короткий безопасный log excerpt с marker (без credentials);
- start/end timestamps и время наблюдения в Europe/Moscow (UTC+3);
- ожидаемый vs фактический outcome, reviewer, verdict и ссылка на owner approval.

Не выдумывать URL/SHA/timestamps; до запуска писать NOT RUN. Ошибка инфраструктуры не доказывает корректность intentional fail-case. Полный пример из templates выполняет pass, затем fail и в целом ожидаемо failed; для независимых verdict нужны отдельно одобренные cases.

## Owner approval gates — до любого реального запуска

- [ ] Подтверждены аккаунт `tishkadront`, Public repository и `main`; нет доступа к production/private ресурсам; `tophqru` исключён.
- [ ] Владелец одобрил подачу **OAuth App**: Name `Woodpecker CI157 Pilot`, Homepage `https://ci.top-hq.ru`, Callback `https://ci.top-hq.ru/authorize`, Device Flow OFF. Здесь форма не подавалась.
- [ ] Владелец отдельно одобрил OAuth authorization и фактические scopes после проверки версии, риска и `WOODPECKER_GITHUB_PUBLIC_ONLY`; это не repo-scoped grant.
- [ ] Любая необходимая будущая работа с credentials отдельно разрешена и выполняется безопасным процессом вне этих файлов; здесь secrets не создавать.
- [ ] Владелец отдельно одобрил активацию ровно этого repository в Woodpecker и webhook activation; проверены callback/webhook URL и границы доступа.
- [ ] Владелец одобрил конкретный активный pipeline, выбранный case, image, runner limits и перемещение .txt в распознаваемую CI-конфигурацию.
- [ ] Владелец отдельно одобрил **first real push**, который вызовет CI, с branch и содержимым; до этого push запрещён.
- [ ] Есть план остановки, ручной контроль и место хранения безопасного evidence.

Регулярный запуск не активировать до успешного G6 и явного отдельного одобрения владельца.
