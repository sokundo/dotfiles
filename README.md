# dotfiles

Curated GNOME desktop configuration for Arch Linux (GNOME Shell 51 / Wayland),
built around the **Graphite-Dark** theme retuned to match **Ghostty's Graphite**
palette (`background = #1c1e22`, `foreground = #d2d6dc`).

Scope is intentionally narrow: GNOME desktop, GTK/libadwaita, the GNOME Shell
theme, Ghostty, and the shell prompt. niri, Waybar, fuzzel, and other
non-GNOME window-manager configs are **not** included.

## Layout

```
home/                                 mirrors $HOME
├── .zshrc .bashrc .bash_profile      shell
├── .gitconfig                        git identity
├── .gtkrc-2.0                        GTK2 settings
├── .config/
│   ├── ghostty/config                Ghostty (Graphite theme, no decoration)
│   ├── starship.toml                 prompt
│   ├── gtk-3.0/settings.ini          GTK3 theme + prefer-dark
│   └── gtk-4.0/
│       ├── settings.ini              GTK4 settings
│       └── gtk.css                   libadwaita background override
└── .themes/Graphite-Dark/            patched theme (shell + GTK3 + GTK4 + GTK2)

dconf/
├── user-theme.txt                    GNOME Shell theme selection
├── rounded-window-corners.txt        rounded-window-corners extension settings
└── interface-theme.txt               GTK/icon/cursor/font keys
```

## Theme changes

Graphite-Dark's backgrounds were moved onto Ghostty's cool-dark ramp.

GTK 3/4 and GTK2 (`gtk-{3,4}.0/gtk{,-dark}.css`, `gtk-2.0/gtkrc`):

| Original  | New       | Where |
|-----------|-----------|-------|
| `#2C2C2C` | `#1C1E22` | windows, dialogs, headerbars, base |
| `#242424` | `#15171A` | sunken surfaces, sidebars, disabled entries |

GNOME Shell (`gnome-shell/gnome-shell.css`):

| Original  | New       | Where |
|-----------|-----------|-------|
| `#2C2C2C` | `#1C1E22` | popup menus, dialogs, notifications, quick toggles |
| `#242424` | `#17191D` | on-screen keyboard, subkeys, screenshot selectors |
| `#212121` | `#15171A` | top panel, panel corner, dash/dock, OSD, workspace switcher |
| `#191919` | `#141619` | overview background |

The `#E0E0E0` accent/border, warning/error colors, and the 2px border on panel
menus are unchanged.

## Install

Assumes Arch/GNOME. Review `gtk-*.0/settings.ini` and `.gtkrc-2.0` first: they
reference fonts, icons, and cursors that may not be installed
(`Adwaita Sans`, `JetBrainsMono Nerd Font`, `Papirus-Dark`,
`capitaine-cursors-light`, `breeze_cursors`). GNOME's own settings override
most of the GTK settings files anyway.

### 1. Shell and app configs

```bash
cd dotfiles
cp -a home/. ~/
```

### 2. Theme and GTK selection

```bash
gsettings set org.gnome.desktop.interface gtk-theme 'Graphite-Dark'
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
gsettings set org.gnome.desktop.interface accent-color 'slate'
```

**User Themes only styles the GNOME Shell.** The GTK theme is a separate
setting, and libadwaita apps ignore both — see below.

Enable the extensions `user-theme@gnome-shell-extensions.gcampax.github.com`
and `rounded-window-corners@fxgn` first, then:

```bash
dconf load /org/gnome/shell/extensions/user-theme/ < dconf/user-theme.txt
```

Or pick *Graphite-Dark* under Tweaks → Appearance → Shell.

### 3. libadwaita apps

Modern GTK4 apps (GNOME Settings, Nautilus, Console, Ghostty chrome) force
`Adwaita-empty` and use libadwaita's built-in palette, so the GTK theme does
not affect them. `home/.config/gtk-4.0/gtk.css` overrides libadwaita's CSS
variables at USER priority:

```css
--window-bg-color: #1C1E22;
--view-bg-color: #1C1E22;
--dialog-bg-color: #1C1E22;
--headerbar-bg-color: #1C1E22;
--sidebar-bg-color: #17191D;
--secondary-sidebar-bg-color: #17191D;
--popover-bg-color: #262A30;
```

### 4. Window corners and border

The rounded corners and the 2px border around app windows are drawn by the
**rounded-window-corners** extension, not by any theme file. Defaults round
windows (radius 15) but draw no border (`border-width=0`,
`skip-libadwaita-app=true`). To match this setup:

```bash
dconf load /org/gnome/shell/extensions/rounded-window-corners-reborn/ \
  < dconf/rounded-window-corners.txt
```

Key values: `border-width=2`, `skip-libadwaita-app=false`, radius `15`,
border color `#E0E0E0`. The dump includes `settings-version=9`; if your
extension version differs, set the values in the extension's Preferences
instead of loading the file.

### 5. Reload

Log out and back in. The shell theme alone can be reloaded live:

```bash
ext=~/.local/share/gnome-shell/extensions/user-theme@gnome-shell-extensions.gcampax.github.com
gsettings --schemadir "$ext/schemas" set org.gnome.shell.extensions.user-theme name ''
gsettings --schemadir "$ext/schemas" set org.gnome.shell.extensions.user-theme name 'Graphite-Dark'
```

## Not included

- **Qt apps** — GTK themes do not apply. Use `qt6ct`/`qt5ct` or Kvantum with
  Window/Base `#1c1e22`, text `#d2d6dc`, highlight `#E0E0E0`.
- **Electron apps** (VS Code, Discord, Spotify) — use their own internal
  themes; only the compositor border applies.
- **Icons, fonts, cursors** — separate installs.
- **Other extensions**, niri/Waybar configs, app data, caches, secrets.

## Notes

- `home/.config/ghostty/config` contains the merged effective settings from
  the original `config.ghostty`. The old `theme = ii-auto` file and the
  auto-generated `ii-auto` theme were stale iNiR output and were dropped.
- `gtk-3.0/colors.css`, `gtk-4.0/colors.css`, `window_decorations.css`, and
  their `assets/` were Breeze leftovers that GTK never loaded; omitted.
- `~/.config/xsettingsd/xsettingsd.conf` is unused in a GNOME session; omitted.
