# Redbee Challenge - Simpsons Quotes API

![Arquitecture](images/Redbee.png)

## Prerequisitos
* Minikube instalado y funcionando.
* Obtener la ip del cluster a través del siguiente comando (se utilizará en los siguientes pasos):
```bash
minikube ip
```
*Intalar addons ingress
```bash
minikube addons enable ingress
```
### Pasos:
1) Clonar este repositorio
```bash
git clone https://github.com/andresciccarelli/reedbe.git
```
2) Posicionarse dentro de la carpeta Resources.
```bash
cd Resources
```
3) Revisar la línea 10 del archivo Resources/simpsons-api-ingress.yaml.
 "- host: simpsons.192-168-49-2.nip.io"
    En caso de que la dirección ip de minikube sea diferente a 192.168.49.2, reemplazar los octetos correspondientes, separados por guiones y guardar los cambios.
4) Crear primero el namespace y luego los demás recursos:
```bash
kubectl create -f simpsons-namespace.yaml
kubectl create -f .
```

![Arquitecture](images/architecture.jpg)

### Test de funcionamiento:

* Dentro de la misma PC, ingresar con un navegador a la dirección.
```bash
http://simpsons.192-168-49-2.nip.io/docs
```
En caso de haber reemplazado este dominio en el paso 3, ingresar la url correcta.

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