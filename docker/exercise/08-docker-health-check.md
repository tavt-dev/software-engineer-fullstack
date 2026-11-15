# Bài 08: Viết script kiểm tra môi trường Docker

Tạo file script trên server:

```bash
cd ~/tavantienk7
nano docker-health-check.sh
```

Nội dung `docker-health-check.sh`:

```bash
#!/bin/bash

echo "user hien tai"
whoami

echo "hostname"
hostname

echo "docker version"
docker --version

echo "danh sach image"
docker images

echo "danh sach container dang chay"
docker ps

echo "danh sach volume"
docker volume ls

echo "danh sach network"
docker network ls

echo "port dang mo tren host"
ss -tuln

echo "log cua container MySQL"
docker logs --tail 50 tavantien_mysql
```

Cấp quyền và chạy:

```bash
cat docker-health-check.sh
chmod +x docker-health-check.sh
ls -l docker-health-check.sh
./docker-health-check.sh
```
