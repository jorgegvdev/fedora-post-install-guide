# 06 — Apps: nativo (dnf) vs Flatpak

## Criterio

| Va nativo (`dnf`) si… | Va Flatpak si… |
|-----------------------|----------------|
| Toca hardware, drivers o el sistema | Es una app gráfica "de usuario" |
| Es herramienta de desarrollo / CLI | Querés versión nueva sin depender de Fedora |
| Necesita ver todo el sistema (Docker, compiladores) | Querés aislarla (chat, música, notas) |
| Es parte del stack de gaming | No necesita integrarse con otras apps |

**Por qué:** una app Flatpak vive en una caja (sandbox) y no ve tus herramientas del sistema. Perfecto para Discord, un problema para un editor que necesita tu `node` o tu `docker`.

## Nativo (dnf)

Instalá de a una, solo lo que uses.

| Paquete | Para qué |
|---------|----------|
| `git` | Control de versiones |
| `steam`, `lutris`, `gamemode`, `mangohud`, `gamescope` | Gaming → [07](07-gaming.md) |
| `docker-ce` | Contenedores → [08](08-desarrollo.md) |
| `code` | VS Code (repo de Microsoft) → [08](08-desarrollo.md) |
| `neovim`, `tmux` | Editor/terminal |
| `ripgrep`, `fd-find`, `bat`, `eza`, `fzf` | Reemplazos modernos de grep/find/cat/ls |
| `btop`, `fastfetch` | Monitor y resumen del sistema |
| `gnome-tweaks` | Ajustes extra de GNOME |

Ejemplo (uno por vez):
```bash
sudo dnf install git
```

### Utilidades (opcionales)

AppImages (necesitan FUSE 2, que Fedora no trae por defecto):
```bash
sudo dnf install fuse fuse-libs
```
Un AppImage corre con tus permisos y sin sandbox: ejecutá solo los que confíes.

Archivos `.rar` (`unrar` viene de RPM Fusion nonfree, ver [04](04-repos-codecs.md)):
```bash
sudo dnf install unrar
```
Archivos `.7z`: revisá primero cuál paquete existe en tu versión con `dnf info 7zip p7zip`, e instalá el que aparezca.

## Flatpak (Flathub)

```bash
flatpak install flathub com.mattjakeman.ExtensionManager
```
Extension Manager: instalar extensiones de GNOME ([10](10-gnome.md)).

```bash
flatpak install flathub com.github.tchx84.Flatseal
```
Flatseal: ver y cambiar permisos de cada app Flatpak.

```bash
flatpak install flathub io.missioncenter.MissionCenter
```
Mission Center: administrador de tareas moderno (CPU, RAM, GPU).

```bash
flatpak install flathub com.discordapp.Discord
```
```bash
flatpak install flathub com.spotify.Client
```
```bash
flatpak install flathub org.telegram.desktop
```
```bash
flatpak install flathub com.obsproject.Studio
```
OBS: es el empaquetado oficial.
```bash
flatpak install flathub org.videolan.VLC
```
```bash
flatpak install flathub com.bitwarden.desktop
```
```bash
flatpak install flathub md.obsidian.Obsidian
```
```bash
flatpak install flathub com.getpostman.Postman
```
```bash
flatpak install flathub io.dbeaver.DBeaverCommunity
```
DBeaver: cliente de bases de datos (PostgreSQL, MySQL, SQL Server).

Gaming (Flatpak solo para estas dos; ver por qué en [07](07-gaming.md)):
```bash
flatpak install flathub com.heroicgameslauncher.hgl
```
```bash
flatpak install flathub net.davidotek.pupgui2
```

## Que ya viene

Firefox, LibreOffice, Fedora Media Writer, Contenedores (Podman). No hace falta reinstalarlos.

## Qué suma a un sistema "limpio y bien usado"

- **Backups reales** (no confundir con snapshots) → [11](11-mantenimiento.md).
- **Snapshots Btrfs** para volver atrás si una actualización rompe algo → [11](11-mantenimiento.md).
- **Firmware al día** (`fwupdmgr`) → [03](03-sistema-base.md).
- **Rutina de limpieza** (paquetes huérfanos, Flatpak sin usar) → [11](11-mantenimiento.md).
- **Un solo origen por app:** no instales la misma app por dnf *y* Flatpak.

## Verificar

```bash
flatpak list --app
```
Lista las apps Flatpak instaladas.

```bash
flatpak run com.github.tchx84.Flatseal
```
Abre Flatseal para revisar permisos.
