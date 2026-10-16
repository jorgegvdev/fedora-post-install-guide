# 07 — Gaming

Requiere [04](04-repos-codecs.md) y [05](../laptop-nvidia/05-nvidia.md) (si tienes NVIDIA).

## Qué es

- **Steam + Proton:** juegos de Windows en Linux (Proton es la capa de compatibilidad de Valve).
- **Lutris / Heroic:** lanzadores para Epic, GOG, Battle.net y otros.
- **GameMode:** sube temporalmente el rendimiento del CPU mientras juegas.
- **MangoHud:** overlay con FPS, temperaturas y uso de GPU/CPU.
- **Gamescope:** micro-compositor para controlar resolución/FPS de un juego.

## Por qué nativo para el stack principal

Steam, MangoHud, GameMode y Gamescope van **nativos** y juntos. Si mezclas Steam Flatpak con MangoHud nativo (o al revés), el overlay puede no cargarse y Proton complicarse. (Criterio de esta guía; Fedora Docs solo documenta Steam vía RPM Fusion.) Heroic y ProtonUp-Qt van por Flatpak porque son apps independientes.

## Pasos

### 1. Instalar lo nativo
```bash
sudo dnf install steam
```
Viene de RPM Fusion nonfree.

```bash
sudo dnf install lutris
```
```bash
sudo dnf install gamemode
```
```bash
sudo dnf install mangohud
```
```bash
sudo dnf install gamescope
```
```bash
sudo dnf install goverlay
```
GOverlay: interfaz gráfica para configurar MangoHud.

### 2. Flatpak
Heroic y ProtonUp-Qt ya figuran en [06](06-apps.md). Si no los instalaste:
```bash
flatpak install flathub com.heroicgameslauncher.hgl
```
```bash
flatpak install flathub net.davidotek.pupgui2
```

### 3. Configurar Steam
1. Abre Steam e inicia sesión.
2. *Configuración → Compatibilidad* → activa **"Habilitar Steam Play para todos los demás títulos"**.
3. Elige **Proton Experimental** o la última versión estable.
4. Si un juego falla, abre **ProtonUp-Qt** y baja **Proton-GE** (versión comunitaria con más parches y códecs). Reinicia Steam y elígelo para ese juego.

### 4. Usar la NVIDIA (solo laptops híbridas)
En una laptop con Intel + NVIDIA, un juego puede correr en la Intel. Ver [07-gaming-nvidia](../laptop-nvidia/07-gaming-nvidia.md).

### 5. Configurar MangoHud (mínimo)
```bash
mkdir -p ~/.config/MangoHud
```
```bash
nano ~/.config/MangoHud/MangoHud.conf
```
Con:
```
fps
gpu_stats
cpu_stats
ram
vram
```
Alternativa visual: abre **GOverlay**.

### 6. Modo de rendimiento (conectado a corriente)
```bash
powerprofilesctl set performance
```
Vuelve a `balanced` con `powerprofilesctl set balanced`.

## Reglas que evitan dolores de cabeza

- **No pongas la biblioteca de Steam en una partición NTFS** (la de Windows). Proton falla o corre mal. Usa el disco de Fedora (Btrfs/ext4).
- **Anticheat:** míralo antes de comprar en [ProtonDB](https://www.protondb.com). Algunos juegos online (Vanguard, ciertos EAC/BattlEye) no funcionan. Para esos, Windows.
- Si un juego da pantalla negra o crashea, prueba otra versión de Proton antes de tocar el sistema.

## Verificar

Verificación de la GPU NVIDIA: [07-gaming-nvidia](../laptop-nvidia/07-gaming-nvidia.md).

```bash
gamemoded -t
```
Test de GameMode: debe terminar con `gamemode seems to be functional`.
