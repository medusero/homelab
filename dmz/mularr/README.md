# Mularr

[aMule](https://github.com/amule-project/amule) es un cliente multiplataforma para la red eD2k/Kad (heredera de eDonkey2000/eMule) para buscar, descargar y compartir ficheros en esta red de igual a igual (P2P).

El despliegue usa [Mularr](https://github.com/joecarl/mularr), que empaqueta el propio *daemon* de aMule junto a una interfaz web moderna y sendos puentes de compatibilidad Torznab / qBittorrent.

## Puertos
- `4662/tcp` y `4672/udp`: puertos P2P eD2k/Kad, publicados al host y redirigidos desde WAN en el router, necesarios para el funcionamiento normal como nodo de la red.
- `8940/tcp`: UI/API web de Mularr, publicada al host. No se expone a WAN.
