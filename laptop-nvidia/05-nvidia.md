# 05 — NVIDIA RTX 4060 (GPU híbrida)

Requiere haber hecho [04](../general/04-repos-codecs.md) (RPM Fusion).

## Qué es

Instalar el driver propietario de NVIDIA. Fedora usa `akmod-nvidia`: compila el módulo del kernel automáticamente en cada actualización de kernel.

## Por qué importa

Sin driver propietario la RTX 4060 usa `nouveau` (lento, sin CUDA, sin buen rendimiento en juegos).

## Secure Boot

Tu Secure Boot está **desactivado**: no hay que firmar nada. Es el camino simple.
Si lo activás en el futuro, hay que firmar el módulo *antes* de instalar el driver (ver al final).

## Pasos

### 1. Sistema al día
```bash
sudo dnf upgrade --refresh
```

### 2. Instalar el driver
```bash
sudo dnf install akmod-nvidia
```

### 3. CUDA, NVENC y `nvidia-smi`
```bash
sudo dnf install xorg-x11-drv-nvidia-cuda
```
Trae `nvidia-smi` y librerías de cómputo/codificación por hardware (útil para OBS, ffmpeg, IA local).

```bash
sudo dnf mark user akmod-nvidia
```
Evita que `dnf autoremove` ([11](../general/11-mantenimiento.md)) borre el driver.

### 4. Esperar la compilación
Pasan hasta ~5 minutos en segundo plano. **No reinicies todavía.**

```bash
modinfo -F version nvidia
```
Si muestra una versión (ej: `580.xx`), está listo. Si dice "not found", esperá y repetí.

### 5. Reiniciar
```bash
sudo reboot
```

### 6. Suspensión/hibernación correctas (recomendado en laptop)
```bash
sudo dnf install xorg-x11-drv-nvidia-power
```
```bash
sudo systemctl enable nvidia-suspend nvidia-resume nvidia-hibernate
```
Evita pantallas negras al suspender/despertar.

### 7. Video acelerado (VA-API sobre NVIDIA)
```bash
sudo dnf install libva-nvidia-driver
```

### 8. Ahorro de energía de la dGPU (opcional)
Permite que la NVIDIA se apague cuando no se usa. Crear el archivo:

```bash
sudo nano /etc/modprobe.d/nvidia.conf
```
Con esta línea:
```
options nvidia NVreg_DynamicPowerManagement=0x02
```
Fuente: [RPM Fusion Optimus](https://rpmfusion.org/Howto/Optimus). Reiniciá para aplicar.

## Usar la NVIDIA a demanda (Optimus)

El escritorio usa la Intel. Para ejecutar algo en la NVIDIA:

- **Desde GNOME:** clic derecho sobre la app → *Launch using Discrete Graphics Card*.
- **Terminal:**
```bash
switcherooctl launch nombre-de-la-app
```
- **Variables de entorno (útil para launch options de Steam, ver [07](07-gaming-nvidia.md)):**
```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only
```

## Verificar

```bash
nvidia-smi
```
Debe mostrar la RTX 4060 y la versión del driver.

```bash
mokutil --sb-state
```
Debe decir `SecureBoot disabled` (tu configuración actual). Si lo activás después, debe decir `enabled` y el módulo NVIDIA tiene que estar firmado.

```bash
switcherooctl list
```
Debe listar dos GPUs (Intel y NVIDIA).

```bash
switcherooctl launch glxinfo | rg "OpenGL renderer"
```
(necesita `sudo dnf install glx-utils`). Debe decir NVIDIA.

## Después de cada actualización de kernel

El módulo se recompila solo al arrancar el kernel nuevo. Si tras actualizar el sistema ves pantalla negra o `nouveau`:

```bash
sudo akmods --force
```
Fuerza la recompilación. Luego `sudo dracut --force` y reiniciá. Plan B: arrancá el kernel anterior desde GRUB ([11](../general/11-mantenimiento.md)).

## Si activás Secure Boot después

1. `sudo dnf install kmodtool akmods mokutil openssl`
2. `sudo kmodgenca -a` — genera la clave local de firma.
3. `sudo mokutil --import /etc/pki/akmods/certs/public_key.der` — pide una contraseña de un solo uso.
4. Reiniciá y, en la pantalla azul MOK, elegí *Enroll MOK* con esa contraseña (el teclado está en QWERTY en esa pantalla).
5. Reiniciá otra vez cuando lo pida.
6. Recién ahí instalá/recompilá `akmod-nvidia`.

Tras una actualización de BIOS/EFI puede hacer falta repetir el `mokutil --import`.

Fuente: [RPM Fusion NVIDIA](https://rpmfusion.org/Howto/NVIDIA).
