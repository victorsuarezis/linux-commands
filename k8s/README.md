# Kubernetes

Website [aquí](https://kubernetes.io/es/).

## Docker

Instalar `docker` [aquí](https://docs.docker.com/engine/install/ubuntu/).

Agregar usuario a docker:
```
$ sudo adduser $(whoami) docker
```

Verificar (no debería ser necesario usar `sudo`):
```
$ docker run hello-world
```

En caso de problemas con permisos reiniciar sesión.

No activar el modo `swarm`, de lo contrario, desactivarlo con:

```
docker swarm leave --force
```

## VirtualBox

Instalar [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads).
```
$ sudo apt install virtualbox
```

## kubectl (kubernetes command line tool)

Instalar `kubectl` [aquí](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

Verificar:
```
$ kubectl version --client
```

## minikube

Instalar `minikube` [aquí](https://minikube.sigs.k8s.io/docs/start/):

Iniciar (intentará usar `docker` por defecto):
```
$ minikube start
```

Para iniciar usando virtualbox (recomandado si estas corriendo docker en modo `swarm`):
```
$ minikube start --vm-driver=virtualbox
```

Eliminar cluster:
```
$ minikube delete --all
```

Ver dashboard:
```
$ minikube dashboard
```

Verificar `kubectl`:
```
$ kubectl cluster-info
```

Ver status de minikube:
```
$ minikube status
```

Ip del cluster:
```
$ minikube ip
```

Obtener objetos soportados en el API:
```
$ kubectl api-resources
```

## Correr app

```
$ kubectl create -f ARCHIVO.yml
```

Ver nodos:
```
$ kubectl get nodes
```

Ver todos los pods:
```
$ kubectl get pods
```

Información de un pod:
```
$ kubectl describe pod <pod>
```

Exponer un puerto (crea un nuevo servicio):
```
$ kubectl expose pod <pod> --name=<service name> --type=<tipo>
```

Ver todos los servicios:
```
$ kubectl get services
```

Información de un servicio:
```
$ kubectl describe service <service>
```

Abrir el servicio expuesto usando minikube (apuntando hacia el nodo):
```
$ minikube service <service>
```

Hacer un forward de un puerto local hacia un pod:
```
$ kubectl port-forward <pod> <puerto local>:<puerto pod>
```

Abrir terminal (o ejecutar algun comando):
```
$ kubectl exec -i --tty <pod> -- bash
```

Ejecutar algun comando:
```
$ kubectl exec <pod> -- <comandos>
```

Ver logs del pod:
```
$ kubectl logs <pod>
```

Correr pod volatil para ejecutar comandos:
```
$ kubectl run -i --tty <nombre> --image=<docker image> --restart=Never -- sh
```

## Compose

Instalar:
```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

Más comandos en [docker](../docker)

## Kompose

Instalar:
```
$ sudo curl -L https://github.com/kubernetes/kompose/releases/download/v1.19.0/kompose-linux-amd64 -o /usr/local/bin/kompose
$ sudo chmod +x /usr/local/bin/kompose
```

Convertir:
```
$ kompose convert
```

## k9s

Instalar `k9s` [aquí](https://k9scli.io/topics/install/)

## Kubectx

Instalar `kubectx` [aquí](https://github.com/ahmetb/kubectx):

```
kubectx
```