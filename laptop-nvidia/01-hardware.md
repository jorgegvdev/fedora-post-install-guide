# 01 — Hardware

Detectado desde Windows con PowerShell.

| Componente | Detalle | Implicancia en Fedora |
|------------|---------|-----------------------|
| Equipo | Lenovo 82XV (línea LOQ) | Firmware actualizable vía LVFS (`fwupd`) |
| CPU | Intel i7-13620H (10 núcleos / 16 hilos) | Soporte completo en kernel. Sin drivers extra |
| RAM | 32 GB DDR5 | Sobra. `zram` (por defecto) alcanza, no toques swap |
| GPU integrada | Intel UHD (Raptor Lake) | Driver `i915`/Mesa en kernel. VA-API con `intel-media-driver` |
| GPU dedicada | NVIDIA RTX 4060 Laptop | Driver propietario por RPM Fusion → [05](05-nvidia.md) |
| Discos | Micron 512 GB + Crucial P3 500 GB (NVMe) | Instalar Fedora en un disco separado de Windows → [02](02-dual-boot.md) |
| Wi-Fi | Realtek RTL8852BE (Wi-Fi 6) | Driver `rtw89` en kernel. Puede fallar tras actualizar → [11](11-wifi-realtek.md) |
| Ethernet | Realtek GbE | Funciona de fábrica |
| Audio / Bluetooth | Realtek (audio + BT) | Funciona de fábrica con PipeWire |

## Por qué importa

- **GPU híbrida (Optimus):** el escritorio corre en la Intel (ahorra batería) y los juegos deben ejecutarse en la NVIDIA a pedido. Es la causa #1 de "mi juego va lento".
- **Wi-Fi Realtek:** es el componente más propenso a dar problemas en Linux. Por eso conviene conservar un kernel anterior como plan B.

## Verificar (ya en Fedora)

```bash
lspci | rg -i "vga|3d|network"
```
Lista GPUs y placa de red (`rg` = ripgrep, ver [08](../general/08-desarrollo.md)). Si aún no lo tenés, usá `lspci | grep -Ei "vga|3d|network"`.

```bash
lsblk -f
```
Muestra discos, particiones y sistemas de archivos. Útil para confirmar cuál disco es Windows.
