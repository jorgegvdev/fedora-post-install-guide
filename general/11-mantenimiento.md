# 11 — Mantenimiento y plan B

## Qué es

Hábitos que mantienen el sistema estable con el tiempo, y qué hacer si algo se rompe.

## Rutina semanal (2 minutos)

```bash
sudo dnf upgrade --refresh
```
Actualiza el sistema.

```bash
flatpak update
```
Actualiza apps Flatpak.

```bash
sudo fwupdmgr get-updates
```
Revisa firmware (aplicar con `sudo fwupdmgr update`).

Después de actualizar el **kernel**, espera unos minutos antes de reiniciar: `akmod-nvidia` compila el módulo ([05](../laptop-nvidia/05-nvidia.md)). Comprueba:
```bash
modinfo -F version nvidia
```

## Limpieza mensual

```bash
sudo dnf autoremove
```
Quita dependencias huérfanas. Requiere haber hecho `sudo dnf mark user akmod-nvidia` ([05](../laptop-nvidia/05-nvidia.md)), o puede borrar el driver NVIDIA.

```bash
flatpak uninstall --unused
```
Quita runtimes Flatpak que ya nadie usa.

```bash
sudo dnf clean all
```
Vacía la caché de DNF.

## Plan B #1: kernel anterior

Fedora conserva varios kernels. Si un kernel nuevo rompe Wi-Fi, NVIDIA o suspensión: en el menú GRUB (mantén `Shift` o `Esc` al arrancar, según tu equipo) elige el kernel anterior en *Advanced options*.

## Plan B #2: Wi-Fi Realtek RTL8852BE inestable

Específico del Lenovo LOQ: ver [11-wifi-realtek](../laptop-nvidia/11-wifi-realtek.md).

## Plan B #3: snapshots Btrfs (opcional)

Fedora usa Btrfs por defecto. Un snapshot es una foto del sistema a la que puedes volver. **No reemplaza un backup**: si el disco muere, se pierde todo.

```bash
sudo dnf install snapper
```
```bash
sudo snapper -c root create-config /
```
Crea la configuración para `/`.

```bash
sudo systemctl enable --now snapper-timeline.timer snapper-cleanup.timer
```
Snapshots automáticos por hora y limpieza de los viejos. Solo actúan sobre subvolúmenes con configuración, por eso va después de `create-config`.

```bash
sudo snapper -c root create --description "antes de actualizar"
```
Snapshot manual (hazlo antes de actualizaciones grandes).

```bash
sudo snapper -c root list
```
Lista los snapshots.

Interfaz gráfica opcional: `btrfs-assistant`. Verifica en su documentación el soporte con la disposición de subvolúmenes por defecto de Fedora (`root` y `home`, no `@`).

> **Timeshift no es compatible** con el layout por defecto de Fedora (usa `@`/`@home`). Usa Snapper.

## Backups (obligatorio si hay datos importantes)

Regla 3-2-1: 3 copias, 2 medios distintos, 1 fuera de tu casa.

```bash
flatpak install flathub org.gnome.DejaDup
```
Déjà Dup: backup simple con interfaz gráfica a disco externo o nube.

## Cambiar de versión de Fedora (44 → 45)

Hazlo cuando el lanzamiento lleve algunas semanas, con extensiones desactivadas ([10](10-gnome.md)), un snapshot reciente y un backup. Sigue la guía oficial: [Upgrading Fedora offline](https://docs.fedoraproject.org/en-US/quick-docs/upgrading-fedora-offline/) o *GNOME Software → Actualizaciones*.

Si usas Docker, revisa luego si [su página para Fedora](https://docs.docker.com/engine/install/fedora/) ya lista la nueva versión.

## Verificar salud del sistema

```bash
systemctl --failed
```
No debe listar servicios fallidos.

```bash
journalctl -p 3 -b
```
Errores graves de este arranque.
