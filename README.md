<h1 align="center">TRAEFIK - Reverse Proxy</h1>

<p align='justify'>
<a href="https://traefik.io)">Traefik</a> is a modern HTTP reverse proxy (RP) and load balancer that makes deploying microservices easy.
</p>

## Install traefik
- Create folders
```bash
DOCKERDIR=/opt/traefik
mkdir -p ${DOCKERDIR}/data/{certs,conf,logs}
touch /opt/traefik/data/conf/acme.json
chmod 600 /opt/traefik/data/conf/acme.json
cd ${DOCKERDIR}
tree -d -L 3 ${DOCKERDIR}
```

- Download files
```bash
DOCKERDIR=/opt/traefik
cd ${DOCKERDIR}
wget https://raw.githubusercontent.com/johann8/traefik/master/.env
wget https://raw.githubusercontent.com/johann8/traefik/master/docker-compose.yml
wget https://raw.githubusercontent.com/johann8/traefik/master/data/conf/dynamic_conf.yml
wget https://raw.githubusercontent.com/johann8/traefik/master/data/conf/traefik.yml
mv dynamic_conf.yml traefik.yml data/conf/
```

- Generate password
```bash
# htpasswd and pwgen must be installed
USER_NAME=user1
PASSWORD_LENGTH=30
TRAEFIK_PASSWORD=$(pwgen -cnsB ${PASSWORD_LENGTH} 1)
echo ${TRAEFIK_PASSWORD}
echo $(htpasswd -nb ${USER_NAME} ${TRAEFIK_PASSWORD}) | sed -e s/\\$/\\$\\$/g

# user1:$$apr1$$ht6O3iSE$$5YxApoPCqeGyMNRsCvXwh/
```

- Set password and user into
```bash
DOCKERDIR=/opt/traefik
cd ${DOCKERDIR}
vi data/conf/dynamic_conf.yml
--------
...
    traefik-auth:
      basicAuth:
        users:
          - "user1:$$apr1$$ht6O3iSE$$5YxApoPCqeGyMNRsCvXwh/" 
...
--------
```

- Customize "docker-compose.yml" and ".env" unsd launch docker container
```bash
DOCKERDIR=/opt/traefik
cd ${DOCKERDIR}
vi .env
vi docker-compose.yml

docker-compose up -d

docker-compose ps
docker-compose logs
```

