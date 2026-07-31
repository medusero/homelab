# slskd

[slskd](https://github.com/slskd/slskd) es un cliente sin interfaz gráfica (headless) para la red Soulseek que expone una API web y una interfaz gráfica propia para buscar, descargar y compartir ficheros en esta red de igual a igual (P2P).

- `50300/tcp`: puerto P2P de Soulseek, publicado al host y redirigido desde WAN en el router, necesario para el funcionamiento normal como nodo de la red.
- `5030/tcp`: UI/API web, publicada al host. Accesible vía LAN. No se expone a WAN.
