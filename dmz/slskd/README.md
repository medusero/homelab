# slskd

[slskd](https://github.com/slskd/slskd) es un cliente sin interfaz gráfica (headless) para la red Soulseek que expone una API web y una interfaz gráfica propia para buscar, descargar y compartir ficheros en esta red de igual a igual (P2P).

- `50300/tcp`: puerto P2P de Soulseek, publicado al host y redirigido desde WAN (`wan-to-dmz-slskd-p2p` en el router) — necesario para el funcionamiento normal como nodo de la red.
- `5030/tcp`: UI/API web, publicada al host. Accesible desde `trusted` vía LAN (`trusted-to-dmz-slskd-ui`) y desde `services` para que Lidarr/Mylar3 consulten la API (`services-to-dmz-slskd-api`). No se expone a WAN.

## Carpetas compartidas
`music`, `books` y `movies` se montan de solo lectura desde el export NFS de Goku (`/mnt/data/share/<carpeta>`), no desde disco local — confirma en la UI de slskd que las tres aparecen como compartidas tras cualquier cambio en `SLSKD_SHARED_DIR`.
