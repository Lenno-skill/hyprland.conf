# Hyprland-Konfiguration

Persönliche Hyprland-/Hyprlock-/Hypridle-Konfiguration mit Noctalia-Shell-Integration.

## Inhalt

| Pfad | Beschreibung |
|------|----------------|
| `hyprland.conf` | Hauptkonfiguration (Monitore, Keybindings, Window Rules) |
| `hypridle.conf` | Idle: Sperre, DPMS, Suspend |
| `hyprlock.conf` | Lockscreen-Layout |
| `hyprlock/colors.conf` | Hyprlock-Farben (Matugen) |
| `noctalia/noctalia-colors.conf` | Fensterrahmen-Farben (wird von Noctalia überschrieben) |
| `scripts/` | Hilfsskripte (Browser-Launcher, Opacity, Statusleiste, …) |
| `local-bin/toggle-laptop-rotation.sh` | Laptop-Display 0°/180° umschalten |

## Installation

```bash
# Backup der bestehenden Config
mv ~/.config/hypr ~/.config/hypr.bak.$(date +%Y%m%d)

# Repo als aktive Hypr-Config
git clone git@github.com:Lenno-skill/hyprland.conf.git ~/.config/hypr

# Zusätzliches Skript (in hyprland.conf referenziert)
install -m755 local-bin/toggle-laptop-rotation.sh ~/.local/bin/
```

Alternativ nur einzelne Dateien kopieren oder per Symlink verknüpfen.

## Abhängigkeiten

- **Hyprland**, **hypridle**, **hyprlock**
- **kitty**, **nemo**, **fuzzel**, **brave** (oder Skript anpassen)
- **Noctalia Shell**: `qs -c noctalia-shell`
- **Screenshot**: `grim`, `slurp`, `swappy`
- **Medien**: `playerctl`
- **Clipboard**: `wl-paste`, `cliphist`
- Optional: `nm-applet`, `notify-send`, `jq` (Laptop-Rotation)

## Hinweise

- `noctalia/noctalia-colors.conf` wird bei Theme-Wechseln von Noctalia neu geschrieben. Die Version im Repo ist ein Snapshot.
- In `hyprlock/colors.conf` steht ein absoluter Wallpaper-Pfad (`$background_image`) — nach Installation anpassen.
- Lockscreen-Befehl: `~/.local/share/quickshell-lockscreen/lock.sh` (separat installiert).

## Struktur auf dem System

Die Config erwartet Pfade unter `~/.config/hypr/` — Keybindings und `exec`-Zeilen verweisen darauf.
