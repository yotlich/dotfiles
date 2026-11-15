# Dotfiles

Идея взята из [статьи Arch Wiki о dotfiles](https://wiki.archlinux.org/title/Dotfiles).

## Установка

**Важно:** `checkout --force` перезаписывает существующие файлы и не делает бэкапов. Если текущие конфиги нужно сохранить — сделать копию заранее.

```bash
git clone --bare https://github.com/yotlich/dotfiles "$HOME/.dotfiles"
dotfiles-boot() { git --git-dir="$HOME/.dotfiles" --work-tree="$HOME" "$@"; }
dotfiles-boot checkout --force
dotfiles-boot sparse-checkout set --no-cone '/*' '!/AGENTS.md' '!/README.md'
dotfiles-boot config --local status.showUntrackedFiles no
```

После `checkout` в `$HOME` уже лежит `~/.local/bin/dotfiles` — дальше используется он (если `~/.local/bin` в `PATH`).

## Обновление

Новые файлы из репозитория приезжают в `$HOME` сами:

```bash
dotfiles pull
```

Если в живом конфиге есть несохранённые правки, `pull` может отказаться их перезаписывать: сначала `dotfiles diff`, потом решить — оформить правку коммитом или отбросить.

## Удаление

Удалить bare-репозиторий и обёртку dotfiles:

```bash
rm -rf ~/.dotfiles
rm -rf ~/.local/bin/dotfiles
```

Команды удаляют только репозиторий и обёртку, конфиги программ не затрагиваются.

## Использование

```bash
# Что изменено
dotfiles

# Сами изменения
dotfiles diff

# После создания файл добавляется явно, без `-A`:
dotfiles add .config/Code/User/snippets/python.json
dotfiles commit -m 'vscode: ...'
dotfiles push
```

## Подробности

Работа с конфигами и репозиторием описана в [AGENTS.md](AGENTS.md).
