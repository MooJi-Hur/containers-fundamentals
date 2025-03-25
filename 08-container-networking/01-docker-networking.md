## Docker Networking

### 기존 드라이버 조회
```bash
docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
............   bridge    bridge    local
............   host      host      local
............   none      null      local
```

```bash
docker network inspect bridge
[
    {
        "Name": "bridge",
...
        "Scope": "local",
        "Driver": "bridge",
...
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",  # subnet
                    "Gateway": "172.17.0.1"
                }
            ]
        },
...
        "Containers": {
            "...": {
...
                "IPv4Address": "172.17.0.2/16",
            }
        }
    }
]
```

### 브릿지를 만들어 컨테이너에 붙이기
- 172.18.0.0/16
```bash
docker network create -d bridge --attachable mynet
```