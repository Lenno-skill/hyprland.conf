# Hyprland-Konfiguration

Persönliche Hyprland-/Hyprlock-/Hypridle-Konfiguration mit [Noctalia Shell](https://github.com/noctalia-dev/noctalia-shell)-Integration.

Repository: [github.com/Lenno-skill/hyprland.conf](https://github.com/Lenno-skill/hyprland.conf)

## Inhalt

| Pfad | Beschreibung |
|------|----------------|
| `hyprland.conf` | Hauptkonfiguration (Monitore, Keybindings, Window Rules) |
| `hypridle.conf` | Idle: Sperre, DPMS, Suspend |
| `hyprlock.conf` | Lockscreen-Layout |
| `hyprlock/colors.conf` | Hyprlock-Farben & Schrift (Matugen) |
| `noctalia/noctalia-colors.conf` | Fensterrahmen-Farben (Snapshot; Noctalia überschreibt bei Theme-Wechsel) |
| `scripts/` | Hilfsskripte (Browser, Opacity, Statusleiste, …) |
| `local-bin/toggle-laptop-rotation.sh` | Laptop-Display 0°/180° umschalten |

## Installation

```bash
# Backup der bestehenden Config
mv ~/.config/hypr ~/.config/hypr.bak.$(date +%Y%m%d)

# Repo als aktive Hypr-Config
git clone https://github.com/Lenno-skill/hyprland.conf.git ~/.config/hypr

# Zusätzliches Skript (in hyprland.conf referenziert)
install -m755 ~/.config/hypr/local-bin/toggle-laptop-rotation.sh ~/.local/bin/
```

Nach Änderungen: `hyprctl reload` (Hyprland) bzw. Neustart von `hypridle`/`hyprlock` bei Bedarf.

## Abhängigkeiten

| Paket / Dienst | Verwendung |
|----------------|------------|
| hyprland, hypridle, hyprlock | Basis |
| kitty, nemo, fuzzel | Terminal, Dateimanager, Launcher (anpassbar) |
| brave / firefox / … | Browser (siehe `scripts/`) |
| quickshell + noctalia-shell | Statusleiste, Lautstärke, Settings |
| grim, slurp, swappy | Screenshot (`Super+Shift+S`) |
| playerctl | Medientasten |
| wl-paste, cliphist | Clipboard-Historie |
| nm-applet | Netzwerk-Tray (optional) |
| jq, notify-send | Laptop-Rotation (optional) |

Lockscreen separat: `~/.local/share/quickshell-lockscreen/lock.sh`

---

## Anpassbare Einstellungen

Die meisten persönlichen Präferenzen stehen in **`hyprland.conf`**. Nach dem Klonen zuerst diese Abschnitte an dein Setup anpassen.

### Programme (`### MY PROGRAMS ###`)

```conf
$terminal = kitty          # z. B. ghostty, alacritty, foot
$fileManager = nemo        # z. B. thunar, dolphin, nautilus
$browser = ~/.config/hypr/scripts/brave-launch.sh
$menu = fuzzel              # z. B. wofi, rofi, vicinae
$mainMod = SUPER            # Alt, CTRL, …
```

**Browser:** In `scripts/` liegen Launcher für Brave, Chromium, Vivaldi und Helium. `$browser` auf das gewünschte Skript zeigen lassen, z. B.:

```conf
$browser = ~/.config/hypr/scripts/vivaldi-launch.sh
```

**Dateimanager per Tastatur:** `Super+E` startet `$fileManager`. **Datei-Manager im Terminal:** `Super+Y` ist fest auf `kitty yazi` — bei anderem Terminal die Zeile anpassen:

```conf
bind = $mainMod, Y, exec, $terminal -e yazi
```

### Monitore (`### MONITORS ###`)

Mit `hyprctl monitors` die Namen auslesen, dann z. B.:

```conf
monitor = eDP-1, 1920x1080@60, 0x0, 1
monitor = HDMI-A-1, preferred, auto, 1
monitor = DP-2, 1920x1080@60Hz, -1920x0, 1
```

Auflösung, Position, Skalierung (`1.25`) und `transform` (0/1/2/3 für Rotation) pro Monitor setzen. Auskommentierte Zeilen dienen als Vorlage.

**Laptop-Rotation:** `Super+R` → `local-bin/toggle-laptop-rotation.sh` — darin `LAPTOP_MONITOR="eDP-1"` anpassen.

### Autostart (`### AUTOSTART ###`)

```conf
exec-once = qs -c noctalia-shell -d    # Noctalia Shell
exec-once = nm-applet &                 # Netzwerk-Tray (entfernen wenn nicht gewünscht)
```

Icon-Theme-Zeile für GTK-Apps (Nemo etc.) — Theme-Namen ändern oder Zeile löschen.

### Umgebungsvariablen (`### ENVIRONMENT VARIABLES ###`)

| Variable | Standard | Anpassung |
|----------|----------|-----------|
| `XCURSOR_SIZE` / `HYPRCURSOR_SIZE` | 18 | Cursor-Größe |
| `XCURSOR_THEME` | Bibata-Modern-Classic | Beliebiges Cursor-Theme |
| `QT_QPA_PLATFORMTHEME` | qt6ct | GTK-Theme für Qt-Apps |

### Look & Feel (`general`, `decoration`, `animations`)

| Einstellung | Standard | Typische Anpassung |
|-------------|----------|-------------------|
| `gaps_in` / `gaps_out` | 0 | Abstände zwischen Fenstern |
| `border_size` | 1 | Rahmenbreite |
| `rounding` | 0 | Eckenradius |
| `active_opacity` / `inactive_opacity` | 0.9 / 0.8 | Transparenz (`Super+O` toggelt live) |
| `blur` enabled/size | true / 3 | Unschärfe unter Fenstern |
| `layout` | dwindle | `master` für Master-Layout |
| `animations` | viele auf `0` | Workspace-/Layer-Animationen aktivieren |

Fensterrahmen-Farben kommen aus **`noctalia/noctalia-colors.conf`** (Noctalia-Theme), nicht direkt aus `hyprland.conf`.

### Eingabe (`input`, `device`)

```conf
kb_layout = de              # z. B. us, de,fr
touchpad { natural_scroll = true }
device { name = epic-mouse-v1; sensitivity = -0.5 }  # name aus hyprctl devices
```

### Keybindings (`### KEYBINDINGS ###`)

| Taste | Aktion | Anpassung |
|-------|--------|-----------|
| `Super+Return` | Terminal | über `$terminal` |
| `Super+E` | Dateimanager | über `$fileManager` |
| `Super+D` | Launcher | über `$menu` |
| `Super+B` | Browser | über `$browser` |
| `Super+Q` | Fenster schließen | — |
| `Super+M` | Hyprland beenden | — |
| `Super+V` | Float toggle | — |
| `Super+F` | Fullscreen | — |
| `Super+O` | Opacity toggle | Skript in `scripts/toggle-opacity.sh` |
| `Super+1`–`0` | Workspaces | Anzahl/Layout in windowrules |
| `Super+Shift+1`–`0` | Fenster verschieben | — |

**Noctalia** (wenn Shell nicht genutzt wird, Block entfernen oder Keys umbelegen):

| Taste | Aktion |
|-------|--------|
| `Super+X` | Session-Menü |
| `Super+L` | Sperren |
| `Super+T` | Dark Mode |
| `Super+N` | Benachrichtigungen |
| `Super+C` | Control Center |
| `Super+S` | Einstellungen |
| `Super+W` | Wallpaper |
| `XF86Audio*` / `XF86MonBrightness*` | Lautstärke/Helligkeit via Noctalia |

### Window Rules (`### WINDOWS AND WORKSPACES ###`)

Apps automatisch auf Workspaces legen — `match:class` an installierte Programme anpassen:

```conf
windowrule = match:class firefox|zen|brave-browser, workspace 2
windowrule = match:class code|cursor, workspace 3
```

Weitere float-Regeln für Dialoge (Rechner, Bluetooth, …) nach Bedarf ergänzen.

### Hypridle (`hypridle.conf`)

| Listener | Timeout | Aktion |
|----------|---------|--------|
| 1 | 300 s (5 min) | Bildschirm sperren |
| 2 | 600 s (10 min) | DPMS aus |
| 3 | 900 s (15 min) | Suspend |

`$lock_cmd` und `$suspend_cmd` bei anderem Lockscreen/Power-Manager anpassen.

### Hyprlock (`hyprlock.conf`, `hyprlock/colors.conf`)

- Layout, Schriftgrößen, Positionen der Labels in `hyprlock.conf`
- Farben und `$font_family` in `hyprlock/colors.conf` (wird von Matugen überschrieben, wenn aktiv)
- **`$background_image`**: absoluten Pfad zum Wallpaper setzen
- Lock-Befehl in `hyprland.conf`: `~/.local/share/quickshell-lockscreen/lock.sh` — durch `hyprlock` ersetzen, falls gewünscht

### Noctalia-Farben

`noctalia/noctalia-colors.conf` wird von Noctalia bei Theme-Wechseln **automatisch neu geschrieben**. Die Version im Repo ist nur ein Snapshot zum Einstieg. Nicht in `hyprland.conf` inline verschieben — die `source`-Zeile am Ende beibehalten.

---

## Struktur auf dem System

Alle Pfade in Keybindings und `exec` zeigen auf `~/.config/hypr/`. Nach dem Klonen direkt dorthin legen (siehe Installation).

## Lizenz

MIT — siehe [LICENSE](LICENSE).
