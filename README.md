# DHCP
Configuracion con Kea

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
