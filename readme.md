## Setup private docker registry with self signed certificate
reference: [link 1](https://forums.docker.com/t/how-to-setup-self-hosted-gitlab-with-container-registry/137245), [link2](https://www.techrepublic.com/article/how-to-deploy-self-hosted-docker-registry-self-signed-certificates/)

1. Generate private key:
```console
cd certs; openssl genrsa 1024 > domain.key; chmod 400 domain.key
```
2. Tạo file config `san.cnf`:
Sửa `IP.1` hoặc `DNS.1` để match với IP/DNS của registry server
3. Generate public key: 
```console
openssl req -new -x509 -nodes -sha1 -days 365 -key domain.key -out domain.crt -config san.cnf
```
4. Deploy với docker compose: `docker compose up -d`
5. Exec vào container `docker-cli` và thử push image vào `registry`:
```console
docker pull alpine
docker tag alpine registry:443/alpine
docker push registry:443/alpine
```