### stats 查看容器的资源利用情况

docker container stats [OPTIONS] [CONTAINER...]

> docker container stats a2e1c8fed55f # 后面的是nexus容器ip
>
> 下面是参数信息

| `CONTAINER ID` and `Name` | the ID and name of the container                             |
| ------------------------- | ------------------------------------------------------------ |
| `CPU %` and `MEM %`       | the percentage of the host’s CPU and memory the container is using |
| `MEM USAGE / LIMIT`       | the total memory the container is using, and the total amount of memory it is allowed to use |
| `NET I/O`                 | The amount of data the container has sent and received over its network interface |
| `BLOCK I/O`               | The amount of data the container has read to and written from block devices on the host |
| `PIDs`                    | the number of processes or threads the container has created |