# Ollama Proxy (Caddy)

[Ollama](https://github.com/ollama/ollama) corre de forma nativa en este servidor, fuera de contenedores, para tener acceso directo a la GPU. Este pod usa [Caddy](https://caddyserver.com) como proxy inverso hacia esa instancia nativa, exponiendo su API a través de Tailscale para poder usarla desde otros dispositivos de tu tailnet sin containerizar el propio Ollama.
El pod usa `Network=host` en vez de una red aislada, ya que es lo que le permite a Caddy alcanzar el Ollama nativo por loopback; ten esto en cuenta si adaptas este quadlet, porque implica que sus contenedores comparten la pila de red del host en lugar de estar aislados como el resto de este repositorio.
