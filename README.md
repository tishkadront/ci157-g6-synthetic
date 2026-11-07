# CI157 G6 Synthetic

Локальный проект файлов для будущего Public repository `tishkadront/ci157-g6-synthetic` и будущего Woodpecker CI на https://ci.top-hq.ru.

**Это synthetic training repository, не production. Сам репозиторий, эти файлы и локальные проверки НЕ являются G6 PASS.** Существование удалённого репозитория и работоспособность CI здесь не проверялись.

## Назначение

Безвредные POSIX sh testcases позволяют в будущем проверить передачу результата между GitHub и Woodpecker: успешный exit 0 и намеренный exit 1. Ни deployment, ни secrets не нужны.

- `testcases/pass/check.sh`: marker `CI157_PASS_CASE`, exit 0.
- `testcases/fail/check.sh`: marker `CI157_FAIL_CASE`, exit 1 (ожидаемый отрицательный результат).
- `templates/woodpecker-pipeline.example.yml.txt`: **неактивный** пример, а не pipeline configuration. В примере сначала pass-case, затем fail-case, поэтому при активации всего примера ожидается failure.
- [G6 matrix и approvals](docs/G6-PASS-FAIL.md).
- [Safety policy](docs/SAFETY.md).

## Будущие критерии

G6 PASS означает: после отдельных owner approvals реальные изолированные тесты дают ожидаемые статусы, статусы привязаны к правильному commit SHA, evidence полно, safety соблюдена. Успешное обнаружение намеренного fail-case является успешной проверкой отрицательного сценария, но сам pipeline остаётся failed.

G6 FAIL означает: неожиданное поведение, false green, неверный SHA/status, утечка, нарушение ограничений или неполное подтверждение. Отсутствие доступа/одобрения — BLOCKED / NOT RUN, не PASS.

Локально можно выполнить `sh testcases/pass/check.sh` и `sh testcases/fail/check.sh`; второй command намеренно возвращает 1. Это не проверяет webhook, forge, runner или G6 end-to-end.

Нет `.github/`, `.woodpecker/` и активного root YAML. Ничего не отправлено и не активировано.
