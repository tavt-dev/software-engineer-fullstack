# Bài 09: Kiểm tra server có web app chạy bằng Docker

Tạo file script trên server:

```bash
cd ~/tavantienk7
nano server-audit-buoi11.sh
```

Nội dung `server-audit-buoi11.sh`:

```bash
#!/bin/bash

echo "Ubuntu co ban"
whoami
hostname
date
uptime
hostname -I
df -h
free -h

echo "process/ port/ service"
ps aux | grep '[d]ocker'
ss -tuln
systemctl status ssh --no-pager
systemctl --failed --no-pager

echo "Docker"
docker images
docker ps
docker logs --tail 50 web-1-tavantien
docker logs --tail 50 tavantien_mysql
docker port web-1-tavantien
docker port tavantien_mysql
docker volume ls
docker network ls

echo "Web app"
curl http://localhost:1510
docker ps | grep web-1-tavantien
docker inspect web-1-tavantien --format '{{json .Mounts}}'
docker inspect tavantien_mysql --format '{{json .Mounts}}'
```

3 rui ro ky thuat:
1. Mat khau root MySQL xuat hien trong lenh va anh chup.
2. Cong database duoc mo tren moi dia chi host theo anh thuc hanh.
3. Chua xac minh co ban sao luu database.

3 viec se lam tiep:
1. Doi mat khau root va tao user rieng cho ung dung.
2. Gioi han ket noi MySQL theo nhu cau su dung.
3. Thiet lap sao luu database va kiem tra khoi phuc.

Cấp quyền và chạy:

```bash
chmod +x server-audit-buoi11.sh
./server-audit-buoi11.sh
cat summary-buoi11.txt
```
