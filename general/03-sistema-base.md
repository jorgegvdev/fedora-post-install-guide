# 03 — Sistema base

## Qué es

Primeros ajustes tras instalar: actualizar, acelerar DNF, firmware y habilitar Flathub completo.

## Por qué importa

Una ISO tiene semanas o meses de antigüedad. Actualizar primero evita instalar sobre paquetes viejos (y problemas con el kernel y el driver NVIDIA después).

## Pasos

### 1. Actualizar todo
```bash
sudo dnf upgrade --refresh
```
`--refresh` fuerza descargar los metadatos nuevos antes de actualizar.

Reiniciá al terminar (hay kernel nuevo).

### 2. Descargas paralelas en DNF
```bash
echo 'max_parallel_downloads=10' | sudo tee -a /etc/dnf/dnf.conf
```
Agrega una línea al final de `/etc/dnf/dnf.conf`: descarga hasta 10 paquetes a la vez. Más rápido, sin riesgo.

### 3. Actualizar firmware (BIOS/SSD/etc.)
Lenovo publica firmware por LVFS.

```bash
sudo fwupdmgr refresh --force
```
Baja el catálogo de actualizaciones.

```bash
sudo fwupdmgr get-updates
```
Lista las disponibles.

```bash
sudo fwupdmgr update
```
Las aplica (puede reiniciar solo; no apagues el equipo durante el proceso).

### 4. Nombre del equipo (opcional)
```bash
sudo hostnamectl set-hostname fedora-loq
```

### 5. Habilitar Flathub completo
Fedora Workstation trae Flathub **filtrado** (solo apps "aprobadas"). Para el catálogo completo:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
Agrega Flathub si no existe (si ya está, no hace nada).

```bash
flatpak remote-modify --no-filter --enable flathub
```
Quita el filtro de Fedora y habilita el repo completo.

### 6. Cosas que ya vienen bien (no tocar)
- **zram** (swap comprimido en RAM): activo por defecto.
- **fstrim.timer** (TRIM de SSD): verificalo abajo.
- **firewalld** y **SELinux enforcing**: dejalos activos. Son parte de lo que hace estable y seguro a Fedora.

## Verificar

```bash
flatpak remotes --columns=name,options,filter
```
La columna `filter` de `flathub` debe estar vacía. (La etiqueta `filtered` en `options` puede aparecer aunque el filtro ya no esté activo; vale la columna `filter`.)

```bash
systemctl status fstrim.timer
```
Debe estar `active (waiting)`. Si no: `sudo systemctl enable --now fstrim.timer`.

```bash
sestatus
```
Debe decir `Current mode: enforcing`.
