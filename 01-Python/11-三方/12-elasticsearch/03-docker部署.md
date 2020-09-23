1. 下载镜像

   ```bash
   docker image pull delron/elasticsearch-ik
   ```

2. 启动容器

   ```bash
   docker run -dti --network=host --name=elasticsearch -v /home/python/Desktop/elasticsearch-2.4.6/config:/usr/share/elasticsearch/config delron/elasticsearch-ik:2.4.6-1.0
   ```

   