# Syncthing

[Syncthing](https://github.com/syncthing/syncthing) es un programa de código abierto para la sincronización continua de ficheros entre dispositivos, de igual a igual (P2P), sin depender de un servidor central ni de servicios en la nube de terceros.
Esta configuración utiliza Tailscale para conectarse de forma segura a tu instancia de Syncthing, garantizando que la interfaz de gestión esté protegida frente a accesos no autorizados y que tu instancia solo sea accesible a través de tu red privada de Tailscale.

## Puertos
- `22000`: necesario para su correcto funcionamiento como nodo de sincronización. El acceso a la UI sigue el patrón habitual, vía Tailscale.
