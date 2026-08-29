# Yazi for Omarchy

<p align="center">
  <a href="https://omarchy.org" target="_blank">
    <img src="https://img.shields.io/badge/Omarchy-4-7aa2f7?style=flat-square&labelColor=1a1b26&logo=archlinux&logoColor=c0caf5"/>
  </a>
  <a href="https://github.com/joaofelipegalvao/omarchy-yazi/blob/main/LICENSE" target="_blank">
    <img src="https://img.shields.io/badge/License-MIT-7aa2f7?style=flat-square&labelColor=1a1b26&logo=github&logoColor=c0caf5"/>
  </a>
  <a href="https://github.com/joaofelipegalvao/omarchy-yazi/releases" target="_blank">
    <img src="https://img.shields.io/github/v/release/joaofelipegalvao/omarchy-yazi?style=flat-square&labelColor=1a1b26&color=7aa2f7&logo=github&logoColor=c0caf5"/>
  </a>
</p>

<div align="center">

**Seamless [Yazi](https://github.com/sxyazi/yazi) theming for [Omarchy](https://omarchy.org)** — Yazi themes that automatically sync with Omarchy theme changes, **with persistent per-theme customization**.

**14 themes · Instant switching**

</div>

<div align="center">
  <table>
    <tr>
      <td align="center">
        <img src="assets/tokyonight.png" width="320" />
        <br/><sub><b>Tokyo Night</b></sub>
      </td>
      <td align="center">
        <img src="assets/catppuccin-macchiato.png" width="320" />
        <br/><sub><b>Catppuccin Macchiato</b></sub>
      </td>
      <td align="center">
        <img src="assets/gruvbox-dark.png" width="320" />
        <br/><sub><b>Gruvbox</b></sub>
      </td>
    </tr>
    <tr>
      <td align="center">
        <img src="assets/kanagawa.png" width="320" />
        <br/><sub><b>Kanagawa</b></sub>
      </td>
      <td align="center">
        <img src="assets/nord.png" width="320" />
        <br/><sub><b>Nord</b></sub>
      </td>
      <td align="center">
        <img src="assets/everforest-dark.png" width="320" />
        <br/><sub><b>Everforest</b></sub>
      </td>
    </tr>
  </table>
</div>

<p align="center">
<em>Yazi updates automatically when switching Omarchy themes with <code>Super + Ctrl + Shift + Space</code></em>
</p>

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/joaofelipegalvao/omarchy-yazi/main/scripts/omarchy-yazi-install.sh | bash
```

Then your current Omarchy theme is applied — live, if your Yazi is recent enough:

> **Hot reload**: with Yazi >= 26.2, Yazi watches the terminal's light/dark scheme and
> **re-applies the theme live in running instances** on every switch — no restart needed.
> Older Yazi only reads the theme on launch:

```bash
# Only needed for Yazi older than 26.2:
killall yazi && yazi
```

> **Security Tip**: Always review scripts before running. For manual installation:

```bash
git clone https://github.com/joaofelipegalvao/omarchy-yazi ~/.local/share/omarchy-yazi
bash ~/.local/share/omarchy-yazi/scripts/omarchy-yazi-install.sh
```

Installer options: `--quiet` (minimal output) · `--force` (regenerate all files) · `--help` · `--version`

## Requirements

* [Omarchy](https://omarchy.org) 4
* [Yazi](https://github.com/sxyazi/yazi) (latest recommended)
* `git`

## How It Works

On every Omarchy theme change:

1. Omarchy writes the theme name to `~/.local/state/omarchy/current/theme.name`
2. The `theme-set` hook triggers `omarchy-yazi-generator`
3. The generator creates/reuses the profile `~/.config/yazi/omarchy-themes/THEME.toml`
4. It updates the symlink `~/.config/yazi/theme.toml` → active profile
5. It clears Yazi's cache — with Yazi >= 26.2 the terminal reports the scheme change and the running instance re-applies the theme **live**

```
Super+Ctrl+Shift+Space → theme-set hook → generator → symlink update → cache clear → live re-theme → ✨
```

Missing a theme template? The generator picks the closest variant automatically (e.g. `catppuccin-*`).

## Supported Themes

<details>
<summary><strong>All 14 stock Omarchy themes are supported out of the box (click to expand)</strong></summary>

**Fully Supported:**

* Catppuccin (macchiato, latte)
* Rose Pine (dawn)
* Tokyo Night (night)
* Gruvbox (dark)
* Everforest (dark)
* Kanagawa (dragon)
* Flexoki (light)
* Nord, Ethereal, Osaka-jade, Hackerman
* Matte-black, Ristretto

</details>

## Customization

Your theme profiles live in `~/.config/yazi/omarchy-themes/` — each theme has its own **persistent profile**:

```bash
# Get current theme
current=$(cat ~/.local/state/omarchy/current/theme.name)

# Edit your profile
nvim ~/.config/yazi/omarchy-themes/$current.toml

# See it applied
killall yazi && yazi
```

**Your changes persist when you switch themes and return!**

### Reset a Profile to Default

```bash
rm ~/.config/yazi/omarchy-themes/tokyo-night.toml
~/.local/bin/omarchy-yazi-generator   # regenerated from template
```

## Verify Installation

```bash
# Generator exists and is executable
ls -la ~/.local/bin/omarchy-yazi-generator

# Hook contains the generator
cat ~/.config/omarchy/hooks/theme-set   # → should list ~/.local/bin/omarchy-yazi-generator

# Symlink points at the current theme's profile
readlink ~/.config/yazi/theme.toml
```

## Troubleshooting

**Theme not updating?**

```bash
chmod +x ~/.config/omarchy/hooks/theme-set
chmod +x ~/.local/bin/omarchy-yazi-generator
cat ~/.local/state/omarchy/current/theme.name   # what Omarchy thinks the theme is
~/.local/bin/omarchy-yazi-generator             # regenerate now
readlink ~/.config/yazi/theme.toml              # what Yazi will use
killall yazi && yazi
```

If the hook file doesn't list `~/.local/bin/omarchy-yazi-generator`, re-run the installer.

## Update

```bash
bash ~/.local/share/omarchy-yazi/scripts/omarchy-yazi-install.sh
```

Pulls the latest templates and regenerates the generator. **Your profiles are never touched.**

## Uninstall

The installer backs up a pre-existing `theme.toml` as `theme.toml.backup.<timestamp>` when it first becomes a symlink — uninstalling restores it for you.

```bash
# Remove everything
curl -fsSL https://raw.githubusercontent.com/joaofelipegalvao/omarchy-yazi/main/scripts/omarchy-yazi-uninstall.sh | bash

# Keep your theme profiles
curl -fsSL https://raw.githubusercontent.com/joaofelipegalvao/omarchy-yazi/main/scripts/omarchy-yazi-uninstall.sh | bash -s -- --keep-configs
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Test thoroughly with multiple themes
4. Submit a pull request

### Adding New Themes

1. Create `themes/YOUR_THEME/theme.toml` in the repository
2. Test with the installer
3. Submit PR with theme file and screenshots

## Acknowledgments

* [@dhh](https://github.com/dhh) — [Omarchy](https://omarchy.org)
* [Yazi](https://github.com/sxyazi/yazi) — Blazing fast terminal file manager
* Community theme creators and contributors

<div align="center">

**[Omarchy](https://omarchy.org)** · **[Yazi](https://github.com/sxyazi/yazi)** · **[Issues](https://github.com/joaofelipegalvao/omarchy-yazi/issues)**

Made with ❤️ for Omarchy users

</div>

## License

MIT License — see [LICENSE](LICENSE) for details