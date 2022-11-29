# Redbee Challenge - Simpsons Quotes API

![Arquitecture](images/Redbee.png)

## Prerequisitos
* Minikube instalado y funcionando.
* Obtener la ip del cluster a través del siguiente comando:
```bash
minikube ip
*Intalar addons ingress
```bash
minikube addons enable ingress
```
### Pasos:
*Clonar este repositorio
```bash
git clone https://
```
* Dentro de la carpeta redbee crear primero en namespace donnde se desplegarn los recursos.
```bash
cd redbee
```
* Revisar la línea 10 del archivo Resources/simpsons-api-ingress.yaml.
- host: simpsons.192-168-49-2.nip.io
En caso de que la dirección ip de minikube sea diferente a 192.168.49.2, reemplazar los octetos correspondientes, separados por guiones.

```bash
kubectl create -f simpsons-namespace.yaml
```

* Los demás recursos se pueden crear ejecutando:
```bash
kubectl create -f .
```
![Arquitecture](images/arquitecture.jpg)

### Opcional:

* Armar un README explicando como realizar el alta de cada recurso de K8S y como acceder a la API.

**Para la resolución se recomienda utilizar [Minikube](https://minikube.sigs.k8s.io/docs/start/), pero se puede utilizar cualquier servicio Kubernetes que tengas de preferencia.**

---

### *Ejemplo: Como ejecutar localmente utilizando solo Docker*

* Levantar la Base de Datos:

```bash
# Ejecutamos la base con una persistencia
cd db

docker run --name simpsons-mysql -p 33306:3306/tcp -v $(pwd)/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Password123 -d mysql:8.0.29
```

* Obtenemos la ip del container (IP_BD)

```bash
docker inspect simpsons-mysql |grep IPAddress
```

* Cargamos los datos:

```bash
mysql -P3306 -h <IP_BD>-u root -p < alta_db.sql
```

* Levantar la API:

```bash
cd api

docker build -t simpsons-quotes:0.1.3 .

docker run --name simpsons-api -e DB_HOST=<IP_BD> -e DB_PORT=3306 -e DB_USER=root -e DB_PASS=Password123 simpsons-quotes:0.1.3
```

* Consultar a la API:

```bash
# Ver frases
curl "http://172.17.0.3:8000/quotes" -s

# Consultar API Docs (desde el navegador)
http://172.17.0.3:8000/docs
```