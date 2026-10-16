# 02 — Dual boot con Windows 11

## Qué es

Fedora y Windows en el mismo equipo, eligiendo al arrancar.

## Por qué importa

El dual boot rompe cosas cuando se descuidan 4 puntos: **cifrado BitLocker**, **inicio rápido de Windows**, **reloj** y **qué disco usas**. Resuélvelos *antes* de instalar.

## Antes de instalar (en Windows)

### 1. Backup
Respalda lo importante a un disco externo o nube. Tocar particiones siempre tiene riesgo.

### 2. Revisar BitLocker / cifrado de dispositivo
Windows 11 Home puede activar "Cifrado de dispositivo" solo. Si está activo y cambias el arranque, Windows puede pedir la clave de recuperación.

Ruta: *Configuración → Privacidad y seguridad → Cifrado de dispositivo*.
Si está activo, guarda la clave de recuperación en https://account.microsoft.com/devices/recoverykey.

### 3. Desactivar inicio rápido e hibernación
Con inicio rápido, Windows deja sus discos "a medio apagar" y Linux no puede montarlos bien.

En una terminal de Windows **como administrador**:

```
powercfg /h off
```
Desactiva hibernación (y con ella el inicio rápido).

### 4. Usar un disco para cada sistema
Tienes dos NVMe. La opción más segura: **Windows en uno, Fedora entero en el otro**. Así el instalador de Fedora no toca particiones de Windows.

Identifica cuál es cuál en *Administración de discos* de Windows (clic derecho en Inicio).

### 5. BIOS
- Entrar a BIOS: tecla `F2` al encender (en LOQ suele ser `F2`; menú de arranque `F12`). Confírmalo en tu equipo.
- Deja **Secure Boot desactivado** (como está ahora). Simplifica NVIDIA.
- Modo de almacenamiento: AHCI (no RAID/Intel RST) para que Linux vea los NVMe.

> **Ojo:** algunos juegos de Windows con anticheat (ej: Valorant/Vanguard) exigen Secure Boot activo. Si lo activas después, hay que firmar el módulo NVIDIA. Ver [05](05-nvidia.md).

## En la instalación

- Usa Fedora Workstation (ISO desde fedoraproject.org, grabada con Fedora Media Writer).
- En el paso de almacenamiento elige **el disco destinatario para Fedora** y deja el sistema de archivos por defecto (**Btrfs**: compresión transparente y snapshots).
- Fedora instala su arranque (GRUB) y detecta Windows. Verifica en la instalación que el disco de Windows **no** esté seleccionado para borrado.

## Después de instalar

### Reloj (Windows y Linux se pelean por la hora)
Windows guarda la hora local en el hardware; Linux guarda UTC. Resultado: la hora cambia entre sistemas.

**Opción recomendada: Windows usa UTC.** El man de `timedatectl` dice que el RTC en hora local "no está totalmente soportado" y que conviene mantenerlo en UTC. En una terminal de Windows **como administrador**:

```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
```
Hace que Windows interprete el reloj de hardware como UTC (método habitual, no figura en Fedora Docs). Después, en Windows, sincroniza la hora desde *Configuración → Hora e idioma*.

**Alternativa simple: Linux usa hora local.**
```bash
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```
Funciona, pero puede dar problemas con cambios de horario de verano.

### Windows no aparece en GRUB
```bash
sudo dnf install os-prober
```
Detecta otros sistemas operativos.

```bash
sudo os-prober
```
Debe imprimir una línea con Windows Boot Manager. Si no imprime nada, GRUB no tiene qué agregar (revisa que Windows esté instalado en modo UEFI).

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```
Regenera el menú de arranque.

Si sigue sin aparecer, revisa `GRUB_DISABLE_OS_PROBER` en `/etc/default/grub`.

### Windows Update cambia el orden de arranque
Pasa a veces. Se arregla desde el menú de BIOS (`F2`) reordenando, o con `efibootmgr`:

```bash
sudo efibootmgr
```
Lista entradas de arranque UEFI.

## Verificar

```bash
lsblk -f
```
Confirma que el disco de Windows no fue modificado.

```bash
timedatectl
```
Revisa la hora y el valor de `RTC in local TZ`: `no` si usaste la opción UTC, `yes` si usaste la alternativa de hora local.
