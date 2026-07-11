# Homelab doméstico en cómodos Quadlets

> THIS IS MY ~~RIFLE~~ HOMELAB. THERE ARE MANY LIKE IT (ON REDDIT) BUT THIS ONE’S MINE

Este repositorio contiene el esqueleto de mi pequeño *homelab*. Está construido sobre Quadlets de Podman, en modo *rootless*. El acceso externo corre a cuenta de [Tailscale](https://tailscale.com).

## Estructura
Este repositorio refleja el contenido de `$HOME/.config/containers/systemd`, con todos sus Quadlets agrupados por directorios y el directorio `secrets` para variables de entorno sensibles (obviamente excluido de Git). El archivo `media-network.network` en la raíz declara la red de Podman que usan los *arr y compañía.

Aparte de esto mi *homelab* reside también en `$HOME/Homelab`. Dentro de ahí la mayoría de los datos persistentes de los contenedores reside en la ruta `data/<quadlet-dir>/data`, con nombres de volúmenes que siguen la pauta `app-config` o `app-data`.

Las excepciones son:
- Los volúmenes de las bases de datos (Postgres, MariaDB o Redis) o similares, que utilizan volúmenes nativos de Podman.
- Los volúmenes y configuraciones de Tailscale, que están en `data/<quadlet-dir>/tailscale-state`, más el archivo `tailscale-serve.json` en la raíz de `data/<quadlet-dir>`.
- Los volúmenes para datos no necesarios para un *backup*, como el directorio `encoded-video` de Immich. Estos están en `var` para fácil exclusión de planes de respaldo.

```
├── data
│   └── quadlet-dir
│       ├── data
│       │   ├── quadlet-config
│       │   └── quadlet-data
│       ├── tailscale-serve.json
│       └── tailscale-state
└── var
    └── quadlet-dir
        └── quadlet-data
```

## Tailscale como único acceso externo

En este entorno todo el acceso externo se hace a través de Tailscale, con el TLS incluido accediendo desde url tipo `https://service.tailnet-name.ts.net`. No hay puertos abiertos en el enrutador, ni *proxy* inverso. Cada Quadlet incluye un *pod* para que Tailscale actúe como sidecar.
