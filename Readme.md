
# Traefik + Consul

![](https://raw.githubusercontent.com/docker-library/docs/a6cc2c5f4bc6658168f2a0abbb0307acaefff80e/traefik/logo.png)![](https://www.datocms-assets.com/2885/1506458593-blog-consul-list.svg?fit=max&fm=jpg&w=1000)


## Project
In this project, we will use:
- Docker
- Docker-Compose
- Traefik LB
- Consul

#### Why Trafik?
Traefik is a powerful tool written in GO.
This tool has the power to intelligently balance loads.
Traefik has 2 ways of configuration:
Statics and Dynamics

In this project we will use dynamic, because it's cooler

[Traefik Link](https://traefik.io "Trafik Link")

#### Why Consul?
Consul is a tool also developed in GO by HashiCorp
With this tool, we can do a number of things:
- DNS
- Service Mesh
- Service Discovery
- KV Store


In this project we are going to use Consul so that traefik consults the taguiated catalog of services and automatically discovers new hosts to balance the load

[Consul Link](https://consul.io "Consul Link")

## How to

First we need to have docker and docker-compose installed on the machine.
To install docker and docker-compose run the commands below:
**Docker:**
```sh
curl -fsSl https://get.docker.com | sh
```
**Docker-Compose**
Debian/Ubuntu
```sh
apt-get install docker-compose
```
Red Hat, CentOs
```sh
yum install docker-compose
```
Now... clone the project
```sh
git clone https://github.com/matheusmgon/traefik-consul.git
```
Access the project folder...

###### Dentro do arquivo "**consul/consul-server/config/server.json**", existe uma linha que define o DNS que o traefik irá usar para balancear a carga, fica a seu critério alterar, mas lembre-se de colocar em seu arquivo host

To start the project, type:

```sh
docker-compose up
```

Unfortunately using the default Traefik image, the interface will not be accessible, but this we solved by registering a traefik service at the consul.
```sh
docker exec -ti traefik curl -XPUT -d '{"ID":"traefik","Name":"traefik","tags":["traefik.enable=true","traefik.http.routers.traefik.entrypoints=web","traefik.http.routers.traefik.rule=Host(`traefik.redelocal`)","traefik.http.routers.traefik.service=api@internal"],"port":80}}' http://consul-server:8500/v1/agent/service/register?replace-existing-checks=true
```

end ...
