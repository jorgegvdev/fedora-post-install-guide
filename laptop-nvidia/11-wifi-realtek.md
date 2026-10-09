# 11 — Plan B: Wi-Fi Realtek RTL8852BE inestable

Específico del Lenovo LOQ (82XV). Complementa [11-mantenimiento](../general/11-mantenimiento.md).

Solo si hay caídas o desconexiones. Primero mirá los errores:
```bash
sudo dmesg | rg rtw89
```
(o `| grep rtw89`).

Verificá que tu kernel acepta los parámetros:
```bash
modinfo rtw89_pci | rg parm
```
Si aparecen `disable_aspm_l1ss` y `disable_clkreq`, creá:
```bash
sudo nano /etc/modprobe.d/rtw89.conf
```
Con:
```
options rtw89_pci disable_aspm_l1ss=y disable_clkreq=y
options rtw89_core disable_ps_mode=y
```
Reiniciá. Es algo que reportan usuarios de la comunidad; puede subir un poco el consumo de batería.
