# Monitorando o Kubernetes com Prometheus e Grafana
### Prometheus + Grafana + Alertas

Hoje irei falar sobre monitoramento e como o Prometheus me ajudou. Qualquer tamanho de cluster, sem um nível mínimo de observabilidade, não sabemos como esta a saúde do cluster.

Minha história começou a mudar quando conheci o Prometheus, no começo achei ele difícil de usar. Então chegou um dia que a minha meta era ter visão das métricas do cluster, pois sem isso é insustentável.

Foi quando, por acaso, achei um projeto público no Github que brilhou meus olhos. Porem, tive que fazer vários ajustes para adaptar as minhas necessidades [PROJETO-ORIGINAL](https://github.com/do-community/doks-monitoring).

A ideia aqui não é ensinar o que é o Prometheus, mas, mostrar como implantar uma stack já constomizada, obtendo dashboards e alertas de um jeito simples. Para mais informações sobre Prometheus recomendo ler a [DOCUMENTAÇÃO](https://prometheus.io/docs/introduction/overview/).

## Pré-requisitos

- Conhecimento em Kubernetes
- Noções em Prometheus


## Ajustes necessários

Como não conheço seu ambiente, recomendo que você faça alguns ajustes antes de executar o projeto:

- Ajustar o 'PersistentVolume' para ficar de acordo com o seu ambiente:
```
prometheus/prometheus-volumes.yml
prometheus/alertmananager-volumes.yml
grafana/grafana-volumes.yml
```

- Ajustar o 'Services', recomendo mudar os dois services abaixo para o tipo loadbalancer, os demais podem ficar como clusterIP:
```
prometheus/prometheus-service.yml
grafana/grafana-service.yml
```

## Guia de configuração

Nesta parte vou mostrar os arquivos mais importantes para manutenção da stack e adaptação dela para o seu ambiente.
Todos os arquivos de configuração do Prometheus, Alertmanager e Grafana foram adaptados para os configmaps facilitando a configuração e implementação.

- No 'prometheus-configmap.yml', estão as configurações do prometheus.yml, rules.yml e alerts.yml. Porem, as maiores mudanças serão no alerts.yml, pois é nele que
fica a configuração dos alertas:

- O 'alertmanager-configmap.yml' é simples, nele é configurado o meio de comunicação do alerta (no meu caso usava Slack):

- Por ultimo, o 'dashboards-configmap.yml' com todos os dashboards salvos em arquivos json. Caso necessário coloque seus próprios dashboard, basta seguir o padrão para funcionar:

## Instalação

Passos ([Repositório](https://github.com/gusta095/prometheus)):
```
git clone https://github.com/gusta095/prometheus.git
cd prometheus
kubectl create namespace promethues
kubectl apply -f prometheus/
kubectl apply -f grafana/
```

- Crie o tunnel e acesse o serviço (caso manteve o clusterIP):
```
kubectl port-forward --namespace prometheus monitoramento-prometheus-${ID-K8S} 9090
kubectl port-forward --namespace prometheus monitoramento-grafana-${ID-K8S} 3000
```


## Conclusão

Eu espero que esse tutorial, ajude, quem busca conhecimento ou esteja implementando uma stack para monitoramento. 

