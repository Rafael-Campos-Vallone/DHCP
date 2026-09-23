# DHCP
Configuracion DHCP utilizando Kea

Instalacion del servicio en las distribuciones de Ubuntu:

sudo apt update && sudo apt install kea-dhcp4-server --y

Cambiar la configuracion de kea, eliminando el contenido del fichero "/etc/kea/kea-dhcp4.conf":

VIM --> vi /etc/kea/kea-dhcp4.conf = 1000dd

Pegar la configuracion y despues editar los valores necesarios para adecuarlos a nuestra red:

```json
{
  "Dhcp4": {
    "interfaces-config": {
      "interfaces": [
        "enp0s8"
      ],
      "dhcp-socket-type": "raw"
    },
    "reservations-global": false,
    "reservations-out-of-pool": true,
    "valid-lifetime": 4000,
    "renew-timer": 1000,
    "rebind-timer": 2000,
    "subnet4": [
      {
        "subnet": "192.168.100.0/24",
        "match-client-id": false,
        "option-data": [
          {
            "name": "routers",
            "data": "192.168.100.1"
          },
          {
            "name": "domain-name-servers",
            "data": "9.9.9.9"
          },
          {
            "name": "ntp-servers",
            "data": "192.168.100.1"
          },
          {
            "name": "domain-name",
            "data": "dominio-100.test"
          }
        ],
        "pools": [
          {
            "pool": "192.168.100.100-192.168.100.199"
          }
        ],
        "reservations": [
          {
            "hw-address": "08:00:27:5c:eb:99",
            "ip-address": "192.168.100.11"
          },
          {
            "hw-address": "08:00:27:2d:8d:a2",
            "ip-address": "192.168.100.12"
          }
        ]
      }
    ],
    "loggers": [
      {
        "name": "*",
        "severity": "DEBUG"
      }
    ]
  }
}
```

Una vez ya tengamos la configuracion en el fichero, debemos reinciar el servicio para que utilize la nueva configuracion y comprobamos que no haya fallos:

systemctl restart kea-dhcp4-server

systemctl status kea-dhcp4-server

GNU/Linux como router:

Activar el reenvío de paquetes:

VIM --> vi /etc/sysctl.conf = /ip_for (buscar matches en el contenido del fichero)

NANO --> nano /etc/sysctl.conf = F6 ip_for (buscar matches en el contenido del fichero)

Descomentamos la linea que dice '#net.ipv4.ip_forward=1' --> 'net.ipv4.ip_forward=1'

Activar el NAT:

VIM --> vi /etc/rc.local

NANO --> nano /etc/rc.local

Pegamos la siguiente configuracion en el fichero:

#!/bin/bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

Concedemos permisos para que utilize la configuracion:

chmod +x /etc/rc.local


