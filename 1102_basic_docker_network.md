# Docker Network introduction
## 1. Shows all network 

```
$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
d41baab1c6b9   bridge             bridge    local
c3c2ba7161db   host               host      local
aeef3df47d0e   none               null      local
```
when installing Docker,it automatically creates 3 networks(bridge, host, none).
Create network in docker 
```
$ docker network create mynet
$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
d41baab1c6b9   bridge             bridge    local
c3c2ba7161db   host               host      local
9a2f9efd2d88   mynet              bridge    local
aeef3df47d0e   none               null      local

$ docker run --net=mynet
```
## 2. docker network inspect bridge
```
[
    {
        "Name": "bridge",
        "Id": "d41baab1c6b96bda48135666bd4a876c912ade1f070bba83b6e0977686618ef2",
        "Created": "2026-05-10T05:58:18.7787197Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {},
        "Containers": {},
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.17.0.0/16": {
                        "IPsInUse": 3,
                        "DynamicIPsAvailable": 65533
                    }
                }
            }
        }
    }
]
```
