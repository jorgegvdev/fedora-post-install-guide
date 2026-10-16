# 10 — GNOME: extensiones y ajustes

## Qué es

Extensiones = mods del escritorio. GNOME es deliberadamente minimalista; unas pocas extensiones lo hacen más cómodo.

## Por qué importa (y el riesgo)

Cada versión mayor de GNOME (Fedora 44 → GNOME 50, Fedora 45 → GNOME 51) puede **romper extensiones** hasta que sus autores las actualicen. Regla: **pocas, populares y mantenidas**. Cada extensión extra es un posible problema tras actualizar.

## Pasos

### 1. Herramientas
```bash
sudo dnf install gnome-tweaks
```
(Si no lo hiciste en [06](06-apps.md)). Y el gestor de extensiones:
```bash
flatpak install flathub com.mattjakeman.ExtensionManager
```
Es la forma actual de explorar e instalar. La integración vieja del navegador con extensions.gnome.org ya no es la vía recomendada.

### 2. Extensiones recomendadas

| Extensión | Para qué | Prioridad |
|-----------|----------|-----------|
| **AppIndicator and KStatusNotifierItem Support** | Íconos de bandeja (Discord, Steam, Telegram) | Imprescindible |
| **Dash to Dock** | Dock siempre visible, tipo escritorio clásico | Alta |
| **Vitals** | CPU/RAM/temperatura en la barra | Media |
| **Caffeine** | Evita que se apague la pantalla (presentaciones, videos) | Media |
| **Clipboard Indicator** | Historial del portapapeles | Media |
| **Just Perfection** | Ajustes finos de la interfaz | Baja |
| **Blur my Shell** | Efectos de desenfoque (solo estético) | Baja |
| **Tiling Shell** | Ventanas en mosaico | Opcional |

**Compatibilidad (verificada en las fuentes):** AppIndicator, Dash to Dock y Dash to Panel tienen soporte confirmado para GNOME 51. Para **Caffeine, Just Perfection y Blur my Shell** no encontré confirmación para GNOME 51: míralas en extensions.gnome.org (columna *Shell Versions*) antes de depender de ellas en Fedora 45.

### 3. Alternativa más estable: extensiones como RPM
Fedora empaqueta algunas. Se actualizan con `dnf` y están probadas con tu versión de GNOME (aunque a veces van un poco atrás del upstream).

```bash
sudo dnf install gnome-shell-extension-appindicator
```
```bash
sudo dnf install gnome-shell-extension-dash-to-dock
```
Activar y listar:
```bash
gnome-extensions list
```
```bash
gnome-extensions enable dash-to-dock@micxgx.gmail.com
```
(usa el identificador exacto que muestre `list`). En Wayland, **cierra sesión y vuelve a entrar** para que cargue.

### 4. Ajustes rápidos por terminal
Botones minimizar/maximizar en las ventanas (GNOME solo muestra cerrar):
```bash
gsettings set org.gnome.desktop.wm.preferences button-layout 'appmenu:minimize,maximize,close'
```
Tema oscuro:
```bash
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
```
Tocar para hacer clic en el touchpad:
```bash
gsettings set org.gnome.desktop.peripherals.touchpad tap-to-click true
```

### 5. Buenas costumbres
- **Antes de actualizar de versión** (44 → 45), desactiva todas las extensiones de terceros y actívalas una por una después.
- Si GNOME falla al iniciar sesión tras una actualización, casi siempre es una extensión: se desactivan desde otra sesión TTY (`gnome-extensions disable <id>`).
- No instales más de ~6-8.

## Verificar

```bash
gnome-extensions list --enabled
```
Muestra las activas.

```bash
gnome-shell --version
```
Confirma tu versión de GNOME para comparar con la compatibilidad de cada extensión.
