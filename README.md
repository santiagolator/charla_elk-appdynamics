# Integrando nuestro toolbox
## Caso de uso Elastic y AppDynamics


Este repositorio sirve de apoyo para los asistentes a la charla **[Caso de uso] Integrando nuestro toolbox: Elastic y AppDynamics**, presentada por la **Comunidad de Elastic Argentina**.

- 📅 30/11/2021 - 19HS (GMT -3)
- 🔗https://community.elastic.co/events/details/elastic-argentina-presents-caso-de-uso-integrando-nuestro-toolbox-elastic-y-appdynamics/

---
## Materiales
- [Slides](https://github.com/santiagolator/charla_elk-appdynamics/blob/main/%5BSHARE%5D%20Presentacion%20Elastic%20+%20APPD.pdf)
- Watchers
	- [Transacciones totales](https://github.com/santiagolator/charla_elk-appdynamics/blob/main/Watchers/watcher_elk-appd-TOTAL.json)
	- [Transacciones totales + ciudad](https://github.com/santiagolator/charla_elk-appdynamics/blob/main/Watchers/watcher_elk-appd-TOTAL+CITY.json)
	- [Transacciones totales + ciudad + categoria prenda](https://github.com/santiagolator/charla_elk-appdynamics/blob/main/Watchers/watcher_elk-appd-TOTAL+CITY+CAT.json)

>  Aclaración: a fines de ejemplificar, utilizamos un índice que forma parte de los data samples
> que brinda Kibana: **kibana_sample_data_ecommerce**

---
## Requisitos para replicar caso de uso

- Instancia de **Elasticsearch + Kibana**
	- En este [link](https://www.elastic.co/es/cloud/elasticsearch-service/signup) pueden suscribirse para un trial de 14 dias de Elastic Cloud.
	- Otra opción es realizar una instalación local del stack. Les dejo un [breve instructivo](#despliegue-elk-en-kubernetes) para el despliegue en Kubernetes (usando Minikube) a través de Helm charts.
- Instancia de **Appdynamics**
	- Desde este [link](https://www.appdynamics.com/free-trial/) pueden obtener un trial por 15 dias de una instancia SaaS de AppDynamics.
- **Machine Agent**
	- El agente se puede descargar una vez tengan la cuenta trial de AppDynamics y puede ser instalado en una VM o de manera local (Linux o Windows). En este [link](https://docs.appdynamics.com/21.2/en/infrastructure-visibility/machine-agent/install-the-machine-agent) les dejo la documentación sobre la instalación.

---

## Despliegue ELK en Kubernetes

Para realizar este despliegue necesitamos contar con un cluster de Kubernetes. Puede ser en la nube o una instalación local con [minikube](https://minikube.sigs.k8s.io/docs/). Además, debemos tener instalado [Helm](https://helm.sh/), ya que vamos a usar prebuilt [charts](https://github.com/bitnami/charts/tree/master/bitnami/elasticsearch) generados por la gente de Bitnami.

> Tener en cuenta que este despliegue es solo con fines de testing y no
> debería ser utilizado en ambientes productivos.

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
