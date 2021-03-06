# Monitorar o kubernetes com Prometheus
### Monitorando um cluster de Kubernetes com o Prometheus e Grafana.

Hoje irei falar sobre monitoramento e como o Prometheus me ajudou com isso, mesmo em um cluster considerado pequeno quando não se tem um certo nível de observabilidade fica difícil saber qual a saúde do cluster.

Minha história começou a mudar quando eu conheci o Prometheus, no começo achei ele extremamente difícil de usar e desisti várias vezes, mais chegou um dia que não dava mais para correr e a falta de visão das métricas do meu cluster ficaram insustentáveis.

Foi quando, por acaso, eu achei um projeto público no Github que brilhou meus olhos e ele estava bem completo porem tive que fazer vários ajustes para funcionar da forma que eu queria [PROJETO-ORIGINAL](https://github.com/do-community/doks-monitoring).

A ideia desde médium não é ensinar o que é o Prometheus, para tais informações eu recomendo a [DOCUMENTAÇÃO](https://prometheus.io/docs/introduction/overview/) a ideia é mostrar de forma simples, como implantar uma stack pronta para usar que vai te oferecer dashboards automáticos e alerta configuravel.

## Pré-requisitos

- Conhecimento em Kubernetes
- Noções em Prometheus


## Ajustes necessários

Como eu não conheço seu ambiente recomendo que você faça alguns ajustes antes de executar o projeto:

- Ajustar o 'PersistentVolume' para ficar de acordo com o seu ambiente.
```
prometheus/prometheus-volumes.yml
prometheus/alertmananager-volumes.yml
grafana/grafana-volumes.yml
```

- Ajustar o 'Services', no meu caso eu recomendo mudar os dois services abaixo para o tipo loadbalancer os demais podem ficar como clusterIP
```
prometheus/prometheus-service.yml
grafana/grafana-service.yml
```

## Guia de configuração

Nesta parte vou mostrar os arquivos mais importantes para manutenção da stack e adaptação dela para o seu ambiente, todos os arquivos de configuração do Prometheus, Alertmanager e Grafana foram adaptados para os configmaps facilitando a manutenção dos mesmos.

- No 'prometheus-configmap.yml', estão todas as configurações do prometheus.yml,rules.yml e alerts.yml, porem o que mais será alterado vai ser o alerts.yml porque é nele que fica a configuração dos alertas:

- O 'alertmanager-configmap.yml' já é mais simples, nele será configurado o meio de comunicação do alerta no meu caso eu usava o slack:

- Por ultimo o 'dashboards-configmap.yml' todos os dashboards estão salvos como arquivos json, e caso queira colocar seus próprios dashboard só siga o padrão que ira funcionar:

## Instalação

[REPOSITÓRIO](https://github.com/gusta095/prometheus)

```
git clone https://github.com/gusta095/prometheus.git
cd prometheus
kubectl create namespace promethues
kubectl apply -f prometheus/
kubectl apply -f grafana/
```

- caso tenha mantido os services como clusterIP:
```
kubectl port-forward --namespace prometheus monitoramento-prometheus-${ID-K8S} 9090
kubectl port-forward --namespace prometheus monitoramento-grafana-${ID-K8S} 3000
```


## Conclusão

Eu sinceramente espero que, caso você esteja precisando de uma stack de monitoramento  ou simplesmente deseje estudar sobre o Prometheus que este projeto te ajude.
