Ниже — финальная схема, которую я бы выбрал.

## Коротко

Твой лучший вариант:

```text
Fork → clone своего fork → установка через uv tool install --editable . → разработка в отдельных ветках → периодически подтягивать upstream
```

Так ты получишь:

```text
глобальную команду из любой папки
+
изолированную установку
+
возможность править код
+
возможность подтягивать обновления автора
```

Проект поддерживает локальную установку через `pip install .` и dev-установку через `pip install -e ".[dev]"`; также в README есть команды `cursor-chronicle` и `search-history`. ([GitHub][1])

---

# 1. Сделай fork

На GitHub открой:

```text
https://github.com/mikhailsal/cursor-chronicle
```

Нажми **Fork**.

---

# 2. Клонируй свой fork

```bash
mkdir -p ~/dev
cd ~/dev

git clone git@github.com:YOUR_USERNAME/cursor-chronicle.git
cd cursor-chronicle
```

Добавь оригинальный репозиторий:

```bash
git remote add upstream https://github.com/mikhailsal/cursor-chronicle.git
git remote -v
```

Должно быть примерно так:

```text
origin    git@github.com:YOUR_USERNAME/cursor-chronicle.git
upstream  https://github.com/mikhailsal/cursor-chronicle.git
```

---

# 3. Установи через uv как глобальный tool

Из папки проекта:

```bash
cd ~/dev/cursor-chronicle
uv tool install --editable .
```

Теперь команда должна быть доступна из любой папки:

```bash
cursor-chronicle --help
search-history --help
```

Если команда не найдена, выполни:

```bash
uv tool update-shell
```

Затем перезапусти терминал.

---

# 4. Как запускать приложение

Например:

```bash
cursor-chronicle --list-projects
cursor-chronicle --list-all
cursor-chronicle --stats
search-history --help
```

---

# 5. Как дорабатывать код

Не работай прямо в `main`. Создавай ветку под каждое изменение:

```bash
cd ~/dev/cursor-chronicle
git checkout -b feature/my-change
```

Меняешь код, потом проверяешь:

```bash
uv run pytest
```

Или просто запускаешь CLI:

```bash
cursor-chronicle --help
```

Так как установка была `--editable`, твои изменения в коде должны сразу использоваться установленной командой.

Если вдруг изменения не подхватились:

```bash
uv tool install --reinstall --editable .
```

---

# 6. Как ставить dev-зависимости

Для разработки внутри проекта можно сделать:

```bash
cd ~/dev/cursor-chronicle
uv sync --extra dev
```

Если `uv sync --extra dev` не сработает из-за структуры проекта, используй:

```bash
uv pip install -e ".[dev]"
```

---

# 7. Как сохранять свои изменения

```bash
git status
git add .
git commit -m "Describe my change"
git push origin feature/my-change
```

Потом можешь сделать Pull Request в свой `main` или смержить локально:

```bash
git checkout main
git merge feature/my-change
git push origin main
```

---

# 8. Как подтягивать обновления автора

Периодически:

```bash
cd ~/dev/cursor-chronicle

git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

Если будут конфликты — решаешь их один раз и продолжаешь.

---

# 9. Как обновить установленную команду после обновлений

После подтягивания изменений:

```bash
uv tool install --reinstall --editable .
```

---

# 10. Что не стоит делать

Не ставь это в системный Python через:

```bash
pip install .
```

Не правь код прямо в скачанной папке без Git.

Не делай все изменения в `main`, если планируешь долго дорабатывать проект.

---

Итоговая рабочая схема:

```bash
# один раз
git clone git@github.com:YOUR_USERNAME/cursor-chronicle.git
cd cursor-chronicle
git remote add upstream https://github.com/mikhailsal/cursor-chronicle.git
uv tool install --editable .

# обычная разработка
git checkout -b feature/some-change
# правишь код
uv run pytest
git add .
git commit -m "Some change"
git push origin feature/some-change

# обновление от автора
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
uv tool install --reinstall --editable .
```

[1]: https://github.com/mikhailsal/cursor-chronicle?utm_source=chatgpt.com "mikhailsal/cursor-chronicle"

