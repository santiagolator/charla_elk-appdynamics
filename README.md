# Integrando nuestro toolbox
## Caso de uso Elastic y AppDynamics


Este repositorio sirve de apoyo para los asistentes a la charla **[Caso de uso] Integrando nuestro toolbox: Elastic y AppDynamics**, presentada por la **Comunidad de Elastic Argentina**.

- 📅 30/11/2021 - 19HS (GMT -3)
- 🔗https://community.elastic.co/events/details/elastic-argentina-presents-caso-de-uso-integrando-nuestro-toolbox-elastic-y-appdynamics/

### Sobre mi
- 💬 Podes preguntarme sobre Elasticsearch, Appdynamics, Monitoring & Observability 🙂
- 📫 Para contactarme: [santiago.lator@gmail.com](mailto://santiago.lator@gmail.com) | [slator@majorkeytech.com](mailto://slator@majorkeytech.com)
- 👷🏼 [Mi perfil laboral](https://www.linkedin.com/in/santiago-lator/)
- 👨‍💻 [Mi Github](https://github.com/santiagolator) 

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
	- Otra opción es realizar una instalación local del stack. Les dejo un [breve instructivo](https://github.com/santiagolator/charla_elk-appdynamics/blob/main/elk_deployment.md) para el despliegue en Kubernetes (usando Minikube) a través de Helm charts.
- Instancia de **Appdynamics**
	- Desde este [link](https://www.appdynamics.com/free-trial/) pueden obtener un trial por 15 dias de una instancia SaaS de AppDynamics.
- **Machine Agent**
	- El agente se puede descargar una vez tengan la cuenta trial de AppDynamics desde la pestaña _Getting Started_ en la Home del Controller y puede ser instalado en una VM o de manera local (Linux o Windows). En este [link](https://docs.appdynamics.com/21.2/en/infrastructure-visibility/machine-agent/install-the-machine-agent) les dejo la documentación sobre la instalación.
	- Además, es necesario iniciar el **listener** del MA. En [esta documentacion](https://docs.appdynamics.com/21.11/en/infrastructure-visibility/machine-agent/extensions-and-custom-metrics/machine-agent-http-listener) pueden encontrar toda la info de como hacerlo.
