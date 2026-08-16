# Dependencies

## Wayland

### Chrome, Electron и т.п.

В зависимости от места установки `~/.local/share/applications/` или
`/usr/share/applications/` открыть файл *.desktop .

Нужно добавить флагу в кажду строку с `Exec`:

```text
Exec=/usr/bin/google-chrome-stable --enable-features=UseOzonePlatform --ozone-platform=wayland %U
```

Проверка для Chrome:

```
chrome://flags
chrome://version
chrome://gpu
```

### Установка с github.

```bash
cd /tmp
wget https://github.com/maintainer/tool_name/releases/download/vx.x.x/filename.tar.gz

tar -xvf filename.tar.gz
mkdir -p ~/.local/bin
mv tool_name ~/.local/bin/
chmod +x ~/.local/bin/tool_name

echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

tool_name --version
```

## Tools

### helix

- [Docs1](https://helix-editor.com/)
- [Docs2](https://helix-editor.vercel.app/)
- [Src](https://github.com/helix-editor/helix)

```bash
snap install --classic helix
hx ~/.config/helix/config.toml
hx ~/.config/helix/languages.toml
```

Проверить, что грамматика есть

```bash
hx --health markdown
```

Покажет статус языка: дерево грамматики, LSP, форматтер. Если грамматики нет:

```bash
hx -g fetch   # скачать tree-sitter грамматики
hx -g build   # скомпилировать
```

#### Автоформатирование

Таблица на момент версии helix 25.07.1

https://github.com/helix-editor/helix/pull/15458

| config.toml | languages.toml | format |
| ----------- | -------------- | ------ |
| true        | true           | true   |
| true        | false          | false  |
| false       | true           | false  |
| false       | false          | false  |

### keyd

- [Src](https://github.com/rvaiya/keyd)

До ubuntu 25.04

```bash
git clone https://github.com/rvaiya/keyd
cd keyd
make && sudo make install
sudo systemctl enable --now keyd
```

С ubuntu 25.04

```bash
sudo apt install keyd
```

### httprunner

- [Docs](https://christianhelle.com/httprunner/)
- [Src](https://github.com/christianhelle/httprunner)

```bash
sudo snap install httprunner
```

### ripgrep

```bash
sudo apt-get install ripgrep
```

### zellij

- [Docs](https://zellij.dev)
- [Src](https://github.com/zellij-org/zellij/)

```bash
curl -L https://github.com/zellij-org/zellij/releases/download/v0.44.3/zellij-x86_64-unknown-linux-musl.tar.gz -o /tmp/zellij.tar.gz
tar -xzf /tmp/zellij.tar.gz -C /tmp
sudo mv /tmp/zellij /usr/local/bin/
zellij --version
mkdir ~/.config/zellij
zellij setup --dump-config > ~/.config/zellij/config.kdl
```

### yazi

- [Docs](https://yazi-rs.github.io)
- [Src](https://github.com/sxyazi/yazi)
- [Nerd Fonts](https://www.nerdfonts.com)

```bash
sudo snap install yazi --classic
```

Установка шрифтов.

```bash
curl -OL https://github.com/ryanoasis/nerd-fonts/releases/latest/download/JetBrainsMono.tar.xz
mkdir -p ~/.local/share/fonts
tar -xf JetBrainsMono.tar.xz -C ~/.local/share/fonts
fc-cache -f -v
```

```bash
# for JSON preview
sudo apt-get install jq
# for video thumbnails
sudo apt install ffmpeg
```

### bat

### glow

Просмотр markdown в терминале.

- [Src](https://github.com/charmbracelet/glow)

Установка с github или snap.

Snap версия более старая.

```bash
sudo snap install glow
```

### delta

- [Docs](https://dandavison.github.io/delta/)
- [Src](https://github.com/dandavison/delta)
- [Releases](https://github.com/dandavison/delta/releases)

```bash
sudo dpkg -i file.deb
```

### 

### mergiraf

### dprint

- [Docs](https://dprint.dev/)

### usql

- [Docs](https://github.com/xo/usql)
- [Src](https://github.com/xo/usql)

```bash
https://github.com/xo/usql/releases/download/v0.21.4/usql-0.21.4-linux-amd64.tar.bz2
tar -xjf usql-0.21.4-linux-amd64.tar.bz2 -C /tmp
sudo mv /tmp/usql /usr/local/bin/
usql --version
```

## lsp, debuger, linter, formatter

### python

#### uv

Это пакетный менеджер

- [Docs](https://docs.astral.sh/uv/)
- [Src](https://github.com/astral-sh/uv)

Всё что устанавливается через `uv tool install` будет лежать в `~/.local/bin/*`

#### basedpyright

- [Docs](https://docs.basedpyright.com)
- [Src](https://github.com/detachhead/basedpyright)

```bash
uv tool install basedpyright
```

#### ruff

- [Docs](https://docs.astral.sh/ruff/)
- [Src](https://github.com/astral-sh/ruff)

```bash
uv tool install ruff
```

#### debugpy

- [Docs](https://github.com/microsoft/debugpy)
- [Src](https://github.com/microsoft/debugpy)

```bash
uv tool install debugpy
```

### codebook

- [Src](https://github.com/blopker/codebook)
- [Docs](https://github.com/blopker/codebook)

Установка с github.

Файл глобальной конфигурации.

```bash
~/.config/codebook/codebook.toml
```

### sql

postgres-language-server https://pg-language-server.com/latest/

### go

https://go.dev/doc/install https://go.dev/gopls/
https://github.com/go-delve/delve
https://github.com/nametake/golangci-lint-langserver

### markdown

#### mpls

- [Src](https://github.com/mhersson/mpls)
- [Docs](https://github.com/mhersson/mpls)

Установка с github.

#### markdown-oxide

https://oxide.md/ https://github.com/Feel-ix-343/markdown-oxide

#### marksman

```bash
# Marksman
wget https://github.com/artempyanykh/marksman/releases/latest/download/marksman-linux-x64 -O ~/.local/bin/marksman
chmod +x ~/.local/bin/marksman
```

### toml

#### taplo

- [Docs](https://taplo.tamasfe.dev)
- [Src](https://github.com/tamasfe/taplo/)

Установка с github.

### javascript

#### typescript-language-server

- [Docs](https://github.com/typescript-language-server/typescript-language-server)
- [Src](https://github.com/typescript-language-server/typescript-language-server)

```bash
npm install -g typescript-language-server typescript
```

### yaml

#### yaml-language-server

- [Docs](https://github.com/redhat-developer/yaml-language-server)
- [Src](https://github.com/redhat-developer/yaml-language-server)

```bash
npm install -g yaml-language-server
```
