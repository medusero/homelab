# Homelab doméstico en cómodos Quadlets

> THIS IS MY ~~RIFLE~~ HOMELAB. THERE ARE MANY LIKE IT (ON REDDIT) BUT THIS ONE’S MINE

Este repositorio contiene el esqueleto de mi pequeño *homelab*. Está construido sobre Quadlets de Podman, en modo *rootless*. El acceso externo corre a cuenta de [Tailscale](https://tailscale.com).

## Disclaimer
Llevo años en esto del auto-alojamiento y el *homelab*, y siempre lo he mantenido desde contenedores Docker y sin más ayuda que documentación, tutoriales y mucha prueba/error, pero cuando migré a Podman se presentaron una serie de inconvenientes que me hizo recurrir a la ayuda de un agente IA.
Estos obstáculos fueron principalmente: hacer funcionar los sidecares Tailscale sin *root*, adaptar los permisos de datos existentes a su nuevo contexto *rootless* y complicaciones añadidas porque SELinux se entrometía en todo.
Si esa ayuda te supone un problema, esto probablemente no sea para ti.

## Estructura
Este repositorio refleja el contenido de `$HOME/.config/containers/systemd`, con todos sus Quadlets agrupados por directorios más un directorio llamado `secrets` para variables de entorno sensibles, y que no está aquí porque obviamente está excluido de Git. El archivo `media-network.network`, en la raíz, declara la red de Podman que usan los *arr y compañía.

Aparte de esto mi *homelab* reside también en `$HOME/Homelab`. Dentro de ahí la mayoría de los datos persistentes de los contenedores reside en la ruta `data/<quadlet-dir>/data`, con nombres de volúmenes que siguen la pauta `app-config` o `app-data`.

Las excepciones son:
- Los volúmenes de las bases de datos (Postgres, MariaDB, Redis...) o similares, que utilizan volúmenes nativos de Podman.
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


## Cómo utilizar uno o varios Quadlets de este repo
Obviamente, necesitas un servidor Linux con Podman instalado y activado. También una cuenta de Tailscale (gratuita).
Mucho cuidado con las rutas para archivos multimedia o personales (películas para Radarr o fotografíás para Immich). Las rutas en los quadlets de aquí son las de mi entorno, y deben ser adaptadas con cuidado.
Los pasos:
1. Clona el repositorio localmente.
2. Copia o mueve la carpeta del servicio que te interese a `$HOME/.config/containers/systemd`.
3. Si la aplicación requiere de variables sensibles (claves, contraseñas), crea el directorio `secrets` en la ruta descrita arriba, y añade los archivos necesarios (los quadlets ya los describen). Es importante que esos archivos tengan los permisos correctos: `chmod 600 secreto`.
4. Crea los directorios necesarios para datos persistentes (Podman no crea los directorios automáticamente, como sí hace Docker). Usaré Baikal como ejemplo: `mkdir -p $HOME/Homelab/data/baikal/{tailscale-state,data/{baikal-config,baikal-data}}`.
5. Avisa al demonio de Systemd de los cambios: `systemctl --user daemon-reload`.
6. Necesitas que tus servicios se levanten incluso si tu usuario no inicia sesión: `loginctl enable-linger $USER`.
7. Levanta el servicio: `systemctl --user start baikal-app.service`.
8. Cada quadlet incluye su propio *healthcheck*. Comprueba que el servicio funcione correctamente antes del siguiente paso: `podman inspect --format='{{.State.Health.Status}}' baikal-app`.
9. Desde un navegador ve a Tailscale y crea una nueva clave. Cuando la tengas, abre con un editor el archivo `baikal-tailscale.container` y busca la línea `Environment=TS_AUTHKEY=`(línea 16). Borra el `CONSUMIDA...` y pon tu clave real.
10. Crea el archivo `tailscale-serve.json` ya mencionado arriba y edítalo con este contenido:
```json
{
  "TCP": {
    "443": {
      "HTTPS": true
    }
  },
  "Web": {
    "baikal.something-something.ts.net:443": {
      "Handlers": {
        "/": {
          "Proxy": "http://localhost:<el-puerto-va-aquí>"
        }
      }
    }
  }
}
```
11. Levanta el contenedor de tailscale: `systemctl --user start baikal-tailscale.service`.
Deberías poder acceder a tu nuevo servicio a través de su dominio en tu *tailnet* (https://baikal.something-something.ts.net). Dependiendo de cómo lo tengas configurado, quizás tengas que aprobarlo desde Tailscale antes de acceder.

## Agradecimiento

Este repositorio, y todo mi *homelab* está fuertemente inspirado en este [repo](https://github.com/tailscale-dev/ScaleTail). Esencialmente, solo he cambiado Docker por Podman.
