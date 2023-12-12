使用 docker exec 指令操作 docker container

開啟 bash 後再使用 mongosh 工具

```sh
docker exec -it MongoDB bash

mongosh -u root -p
```

直接開啟 mongosh 工具 

```sh
docker exec -it MongoDB mongosh -u root -p
```