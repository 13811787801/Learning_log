```shell
docker search nacos/nacos-server

docker pull nacos/nacos-server:1.0.0

docker run --env MODE=standalone --name nacos -d -p 8848:8848 nacos/nacos-server
```

