# slskd

Soulseek es una red P2P centrada en el intercambio de música, con un servidor central que indexa usuarios y búsquedas mientras las transferencias de ficheros ocurren directamente entre clientes.

[slskd](https://github.com/slskd/slskd) es un cliente sin interfaz gráfica (headless) para esta red que expone una API web y una interfaz gráfica propia para buscar, descargar y compartir ficheros.

- `50300/tcp`: puerto P2P de Soulseek, publicado al host y redirigido desde WAN en el router, necesario para el funcionamiento normal como nodo de la red.
- `5030/tcp`: UI/API web, publicada al host. Accesible vía LAN. No se expone a WAN.
