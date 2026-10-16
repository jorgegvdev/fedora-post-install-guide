# 04 — Repositorios (RPM Fusion) y codecs

## Qué es

**RPM Fusion** es el repositorio de la comunidad que tiene lo que Fedora no incluye por licencias/patentes: driver NVIDIA, Steam, codecs completos de video.

- `free`: software libre con restricciones de patentes (ffmpeg completo).
- `nonfree`: software cerrado (NVIDIA, Steam, `intel-media-driver`).

## Por qué importa

Sin esto: sin NVIDIA propietario, sin Steam, y videos H.264/H.265 que no se reproducen bien o no se aceleran por hardware.

## Pasos

### 1. Habilitar RPM Fusion
```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```
`$(rpm -E %fedora)` se reemplaza solo por tu versión (44, 45…).

```bash
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### 2. Metadatos para tiendas gráficas (GNOME Software)
```bash
sudo dnf install rpmfusion-free-appstream-data
```
```bash
sudo dnf install rpmfusion-nonfree-appstream-data
```

### 3. Códec H.264 de Cisco
```bash
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
```
Habilita el repo (Fedora 41+).

```bash
sudo dnf install mozilla-openh264
```
Para que Firefox reproduzca H.264.

### 4. ffmpeg completo
Fedora trae `ffmpeg-free` (recortado). Lo cambiamos por el completo:

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```
`swap` reemplaza un paquete por otro. `--allowerasing` permite quitar el reemplazado.

### 5. Códecs de GStreamer
```bash
sudo dnf install @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```
Instala el grupo multimedia. `install_weak_deps=False` evita paquetes opcionales innecesarios; el `--exclude` omite un plugin que no hace falta.

### 6. Video acelerado por hardware (Intel)
```bash
sudo dnf install intel-media-driver
```
Habilita VA-API para la iGPU Intel (decodificar video con poco consumo). La parte NVIDIA va en [05](../laptop-nvidia/05-nvidia.md).

## Verificar

```bash
sudo dnf install libva-utils
```
```bash
vainfo
```
Debe listar varios `VAProfile…`. Si da error, reinicia y reintenta.

```bash
rpm -q ffmpeg
```
Debe mostrar `ffmpeg-…` (no `ffmpeg-free`).

> Nota: RPM Fusion no es oficial de Fedora. Según tu país, ciertos códecs pueden tener restricciones de patentes.
