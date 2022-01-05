# Docker安装MinIO

## 拉取镜像
``` bash
docker pull minio/minio:RELEASE.2021-06-17T00-10-46Z
```

## 启动镜像
``` bash
docker run -p 9000:9000 --name minio \
  -d --restart=always \
  -e "MINIO_ACCESS_KEY=minio" \
  -e "MINIO_SECRET_KEY=minio" \
  -v /mydata/minio/data:/data \
  -v /mydata/minio/config:/root/.minio \
  minio/minio:RELEASE.2021-06-17T00-10-46Z server /data
```

> 注意事项
防火墙开放 9000 端口