# Despliegue ELK en Kubernetes

Para realizar este despliegue necesitamos contar con un cluster de Kubernetes. Puede ser en la nube o una instalación local con [minikube](https://minikube.sigs.k8s.io/docs/). Además, debemos tener instalado [Helm](https://helm.sh/), ya que vamos a usar prebuilt [charts](https://github.com/bitnami/charts/tree/master/bitnami/elasticsearch) generados por la gente de Bitnami.

> Tener en cuenta que este despliegue es solo con fines de testing y no
> debería ser utilizado en ambientes productivos.

> Este procedimiento se realizo utilziando minikube v1.23.2 para Windows, en terminal de Powershell v7.2.0

1. Antes de iniciar, es posible que necesitemos alocar ciertos recursos
```powershell
minikube config set cpus 4 
minikube config set memory 8192
```
2.  Agregamos el repo de Bitnami
```powershell
    helm repo add bitnami https://charts.bitnami.com/bitnami
```

3.  Desplegar **Elasticsearch**

```powershell
helm install elasticsearch --set master.replicas=3,coordinating.service.type=LoadBalancer bitnami/elasticsearch
```
4. Verificamos el despliegue con el siguiente comando:

```powershell
kubectl get all
```
Deberíamos ver los 7 pods con status `Running`

5.  Desplegar **Kibana** *(vinculado a la instancia de Elasticsearch)*

```powershell
helm install kibana elastic/kibana --set elasticsearchHosts=http://elasticsearch-coordinating-only:9200 --set service.type=LoadBalancer
```

6. Nuevamente, verificamos el despliegue:

```powershell
kubectl get all
```
Deberíamos ver los 1 pod con status `Running`

7.  En **minikube**, para exponer los puertos localmente es necesario el siguiente comando, el cual toma la terminal (si cancelamos el proceso el tunnel se cierra).

```powershell
minikube tunnel
```

8.  Con el tunnel abierto
	- Verificar que **Elasticsearch** responda correctamente
	 ```powershell
	curl http://<IP_ASIGNADA>/_cluster/state?pretty
	```
	- podemos ir a [http://<IP_ASIGNADA>:5601](http://%3CIP_ASIGNADA%3E:5601)  para acceder a **Kibana** en nuestro navegador.

> IP_ASIGNADA por lo general es **localhost** o **127.0.0.0**, revisar con `kubectl get all`
