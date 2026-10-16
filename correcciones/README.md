# Correcciones post-auditoría

Para quien ya siguió la guía. Parte A: comandos a correr en el sistema. Parte B: cambios de texto en la guía.

## Parte A — Correr en Fedora

### 1. Proteger `akmod-nvidia` de `dnf autoremove` (importante)
Sin esto, `dnf autoremove` (rutina mensual de [11](../general/11-mantenimiento.md)) puede borrar el driver.

```bash
sudo dnf mark user akmod-nvidia
```
Lo marca como instalado por el usuario, así autoremove no lo toca.

Verificar:
```bash
dnf repoquery --userinstalled akmod-nvidia
```
Debe listar `akmod-nvidia`.

### 2. Actualizar nvm a v0.40.8
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
```
Reinstalar sobre la existente actualiza nvm; tus versiones de Node se conservan. Abre una terminal nueva y verifica:
```bash
nvm --version
```

### 3. Variable de DeepSeek (solo si usas `claude-ds`)
La doc de DeepSeek usa `ANTHROPIC_API_KEY`; tu alias usa `ANTHROPIC_AUTH_TOKEN`. Si `claude-ds` funciona, no toques nada. Si falla la autenticación, edita la línea del alias en `~/.bashrc`:

```bash
alias claude-ds='ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic ANTHROPIC_API_KEY="$DEEPSEEK_API_KEY" claude'
```
Recarga con `source ~/.bashrc`.

## Parte B — Cambios en la guía

| Archivo | Cambio |
|---|---|
| `05-nvidia.md` (tras paso 3) | Agregar `sudo dnf mark user akmod-nvidia` con la nota: "evita que `dnf autoremove` borre el driver" |
| `11-mantenimiento.md` (Limpieza mensual) | Aviso bajo `dnf autoremove`: requiere haber hecho el `dnf mark user akmod-nvidia` de [05](../laptop-nvidia/05-nvidia.md) |
| `08-desarrollo.md:73` | `v0.40.5` → `v0.40.8` |
| `09-ia.md:65` | `ANTHROPIC_AUTH_TOKEN` → `ANTHROPIC_API_KEY` (según doc de DeepSeek) |
| `09-ia.md:10` | Reemplazar "según prensa técnica, 07/2026" por la fuente oficial: [Claude Desktop on Linux](https://code.claude.com/docs/en/desktop-linux) ("Fedora and RHEL: only Debian-based distributions are supported today") |
| `09-ia.md` (Claude Code) | Mencionar alternativa oficial: repo `dnf` de Claude Code ([setup](https://code.claude.com/docs/en/setup#install-with-linux-package-managers)). Sin auto-update: `sudo dnf upgrade claude-code` |
| `00-indice.md` | Agregar fallback `03/11` de Fedora 45 |

## Parte C — Segunda ronda (opcional / diagnóstico)

Ninguno es obligatorio.

### 1. Reloj en UTC (opcional)
Tu Fedora usa `set-local-rtc 1`, que funciona. El man de `timedatectl` recomienda UTC porque la hora local da problemas con el horario de verano. Migra solo si ves la hora corrida.

En Windows, terminal **como administrador**:
```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
```
En Fedora:
```bash
sudo timedatectl set-local-rtc 0
```
Verificar: `timedatectl` debe decir `RTC in local TZ: no`. Después sincroniza la hora en Windows (*Configuración → Hora e idioma*).

### 2. Chequeo de Flathub (diagnóstico)
```bash
flatpak remotes --columns=name,options,filter
```
La columna `filter` de `flathub` debe estar vacía. La etiqueta `filtered` en `options` puede ser un resto sin efecto.

### 3. Windows no aparece en GRUB (solo si pasa)
```bash
sudo os-prober
```
Si no imprime nada, revisa que Windows esté instalado en modo UEFI.

## Verificado sin cambios en el sistema
- `__NV_PRIME_RENDER_OFFLOAD` (con `__`) es la forma que documenta NVIDIA.
- Timeshift no es compatible con el layout `root`/`home` de Fedora; Snapper sí.
- Flujo de `fwupdmgr`, sintaxis DNF5 y upgrade offline correctos.
- Secure Boot: sin cambios en tu sistema (está desactivado).

## Sin verificar
Las páginas en inglés de docs.fedoraproject.org (bloqueadas por Anubis) y MangoHud/GameMode, sin doc oficial de Fedora.
