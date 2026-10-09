# Fedora post-instalación — Lenovo LOQ (82XV)

Guía en español para dejar Fedora Workstation (GNOME) estable y productivo en un Lenovo LOQ (i7-13620H, RTX 4060), en dual boot con Windows 11 y con Secure Boot desactivado.

**Empezá por el [índice](00-indice.md).**

## Qué cubre

Dual boot, sistema base, RPM Fusion y codecs, NVIDIA híbrida (Optimus), apps, gaming, entorno de desarrollo, herramientas de IA, GNOME y mantenimiento. Cada archivo sigue el formato *Qué es → Por qué importa → Pasos → Verificar*.

## Estado

- Versiones: Fedora 44 (estable) y 45 (lanzamiento previsto 20/10/2026).
- Auditada contra documentación oficial de RPM Fusion, Docker, Claude Code, DeepSeek, OpenCode, NVIDIA y man pages. Lo corregido está en [correcciones](correcciones/README.md).
- No verificado contra Fedora Docs en inglés (bloqueado al auditar) ni contra doc oficial para MangoHud/GameMode.

## Aviso

Guía personal, no oficial. Los comandos están pensados para este equipo; revisalos antes de ejecutarlos en otro. Hacé backup antes de tocar particiones.
