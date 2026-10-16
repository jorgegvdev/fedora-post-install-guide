# Fedora post-instalación — Lenovo LOQ (82XV)

Guía para dejar Fedora Workstation (GNOME) estable y productivo en este equipo, en **dual boot con Windows 11**, con **Secure Boot desactivado**.

## Cómo usar esta guía

- Sigue los archivos **en orden**. Cada uno es independiente, pero el orden evita problemas (ej: sin RPM Fusion no hay NVIDIA ni codecs).
- Cada comando va solo, con su explicación. Copia y pega uno por uno.
- Estructura de cada archivo: **Qué es** → **Por qué importa** → **Pasos** → **Verificar**.
- Los comandos usan `$(rpm -E %fedora)` donde hace falta, así sirven igual en Fedora 44 y 45.

## Orden

| # | Archivo | Para qué |
|---|---------|----------|
La guía se divide en dos carpetas: **`general/`** sirve para cualquier Fedora Workstation; **`laptop-nvidia/`** es para laptops híbridas Intel + NVIDIA (con partes específicas del Lenovo LOQ marcadas).

| # | Archivo | Para qué |
|---|---------|----------|
| 01 | [laptop-nvidia/hardware](laptop-nvidia/01-hardware.md) | Qué tiene la máquina y qué implica (LOQ) |
| 02 | [laptop-nvidia/dual-boot](laptop-nvidia/02-dual-boot.md) | Convivir con Windows sin romper nada |
| 03 | [general/sistema-base](general/03-sistema-base.md) | Actualizar, DNF, firmware, Flathub |
| 04 | [general/repos-codecs](general/04-repos-codecs.md) | RPM Fusion, codecs, video acelerado |
| 05 | [laptop-nvidia/nvidia](laptop-nvidia/05-nvidia.md) | Driver RTX 4060 + GPU híbrida |
| 06 | [general/apps](general/06-apps.md) | Qué va nativo (dnf) y qué va Flatpak |
| 07 | [general/gaming](general/07-gaming.md) | Steam, Proton, Lutris, MangoHud, GameMode |
| 07b | [laptop-nvidia/gaming-nvidia](laptop-nvidia/07-gaming-nvidia.md) | Forzar la NVIDIA en juegos (laptop híbrida) |
| 08 | [general/desarrollo](general/08-desarrollo.md) | Git, nvm/Node, Docker, VS Code, terminal |
| 09 | [general/ia](general/09-ia.md) | Claude Code, OpenCode, DeepSeek |
| 10 | [general/gnome](general/10-gnome.md) | Extensiones y ajustes de escritorio |
| 11 | [general/mantenimiento](general/11-mantenimiento.md) | Rutina, snapshots, backups, plan B |
| 11b | [laptop-nvidia/wifi-realtek](laptop-nvidia/11-wifi-realtek.md) | Plan B: Wi-Fi Realtek RTL8852BE (LOQ) |

## Qué versión instalar

- **Fedora 44** es la estable actual (salió el 28/04/2026, GNOME 50).
- **Fedora 45** (GNOME 51) tiene fecha objetivo **20/10/2026** (fallback 27/10 y 03/11).
- Recomendación: si instalas **ahora**, usa 44. Si puedes esperar unos días, 45. Extensiones de GNOME y driver NVIDIA suelen tardar un poco en estabilizarse con una versión nueva; esperar 2-3 semanas tras un lanzamiento es una opinión de comunidad, no una regla oficial.

## Regla de oro: nativo vs Flatpak

- **Nativo (dnf):** lo que toca hardware, drivers, el sistema o herramientas de desarrollo (Steam, MangoHud, Docker, git, VS Code).
- **Flatpak (Flathub):** apps gráficas de usuario que se benefician de estar aisladas y actualizadas (Discord, Spotify, OBS, Obsidian).
- **Recomendación de esta guía (no es doc oficial): no mezcles el stack de gaming.** Steam nativo + MangoHud nativo + GameMode nativo. Mezclar versiones Flatpak/nativo puede romper overlays y Proton.

Detalle completo en [06-apps](general/06-apps.md).

## Fuentes principales

- RPM Fusion: [Configuration](https://rpmfusion.org/Configuration), [NVIDIA](https://rpmfusion.org/Howto/NVIDIA), [Optimus](https://rpmfusion.org/Howto/Optimus), [Multimedia](https://rpmfusion.org/Howto/Multimedia)
- Docker: [Install on Fedora](https://docs.docker.com/engine/install/fedora/)
- Claude Code: [Setup](https://code.claude.com/docs/en/setup)
- DeepSeek: [Anthropic API](https://api-docs.deepseek.com/guides/anthropic_api)
- OpenCode: [Providers](https://opencode.ai/docs/providers/)
- Fedora Docs: [Gaming](https://docs.fedoraproject.org/en-US/gaming/)

> Los paquetes y versiones cambian. Si un comando falla, revisa primero la fuente oficial enlazada en esa sección.
