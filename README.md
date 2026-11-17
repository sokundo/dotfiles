# dotfiles

My gnome dotfiles

## Screenshot
<img src="./Screenshot/screenshot" alt="screenshot">

## Software
- Ghostty ternimal with graphite theme and window decorations removed in config.
- zsh shell + oh my zsh and starship

## GTK theme
- Graphite
  
## Gnome Shell Extensions
- Dash to dock
- Rounded window corners reborn
- transparent top bar (adjustable transparency)
- user themes
- better tray icons
- WACK - sonoma lockscreen

## Layout

```
home/                                 mirrors $HOME
├── .zshrc .bashrc .bash_profile      shell
├── .gitconfig                        git identity
├── .gtkrc-2.0                        GTK2 settings
├── .config/
│   ├── ghostty/config                Ghostty (Graphite theme, no decoration)
│   ├── fastfetch/config.jsonc        Fastfetch config
│   ├── starship.toml                 prompt
│   ├── gtk-3.0/settings.ini          GTK3 theme + prefer-dark
│   └── gtk-4.0/
│       ├── settings.ini              GTK4 settings
│       └── gtk.css                   libadwaita background override
└── .themes/Graphite-Dark/            edited theme (shell + GTK3 + GTK4 + GTK2) (i edited this theme instead of switchinig to normal graphite theme, it was 1am, don't do the same mistake)

dconf/
├── user-theme.txt                    GNOME Shell theme selection
├── rounded-window-corners.txt        rounded-window-corners extension settings
└── interface-theme.txt               GTK/icon/cursor/font keys
```

