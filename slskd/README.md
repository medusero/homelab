# slskd

[slskd](https://github.com/slskd/slskd) es un cliente sin interfaz gráfica (headless) para la red Soulseek que expone una API web y una interfaz gráfica propia para buscar, descargar y compartir ficheros en esta red de igual a igual (P2P).
Esta configuración utiliza Tailscale para conectarse de forma segura a tu instancia de slskd, garantizando que la interfaz esté protegida frente a accesos no autorizados y que tu instancia solo sea accesible a través de tu red privada de Tailscale.

## Puertos
Mientras que el resto de aplicaciones de este repositorio no expone puertos al confiar su acceso externo a Tailscale, slskd es una de las excepciones; expone el puerto 50300 necesario para su correcto funcionamiento como nodo p2p. El acceso a la UI sigue el patrón habitual, vía Tailscale.
