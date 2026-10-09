# 07 — Gaming en laptop híbrida (NVIDIA)

Complementa [07-gaming](../general/07-gaming.md). Requiere [05](05-nvidia.md).

## Usar la NVIDIA (muy importante en laptop)
Sin esto, un juego puede correr en la Intel.

**Opción simple:** abrí Steam con clic derecho → *Launch using Discrete Graphics Card*.

**Opción por juego:** en Steam, clic derecho al juego → *Propiedades → Opciones de lanzamiento*:
```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only gamemoderun mangohud %command%
```
- `%command%` es el juego.
- `gamemoderun` activa GameMode.
- `mangohud` muestra el overlay.
- Las 3 variables `__…` fuerzan la NVIDIA.

## Verificar

```bash
nvidia-smi
```
Con un juego abierto, debe aparecer el proceso del juego en la lista.

MangoHud: dentro del juego, el overlay muestra el nombre de la GPU. Debe decir NVIDIA (o `RTX 4060`).
