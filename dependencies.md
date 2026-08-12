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

```bash
go install github.com/charmbracelet/glow/v2@latest
```

Snap версия проблемная, так как полностью в песочнице.

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

# usql

- [Docs](https://github.com/xo/usql)
- [Src](https://github.com/xo/usql)

```bash
https://github.com/xo/usql/releases/download/v0.21.4/usql-0.21.4-linux-amd64.tar.bz2
tar -xjf usql-0.21.4-linux-amd64.tar.bz2 -C /tmp
sudo mv /tmp/usql /usr/local/bin/
usql --version
```

## lsp

### sql

postgres-language-server https://pg-language-server.com/latest/

### go

https://go.dev/doc/install https://go.dev/gopls/
https://github.com/go-delve/delve
https://github.com/nametake/golangci-lint-langserver

### markdown

https://oxide.md/ https://github.com/Feel-ix-343/markdown-oxide
