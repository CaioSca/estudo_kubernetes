# Estudo Kubernetes

INFRA BÁSICA

  - Pod é a unidade mínima do kubernetes. Ele é montado como uma camada de abstração sobre o container
  - Pods tem IPs únicos, ou seja, quando pod morre, é criado um novo, com novo IP
  - Serviço existe para definir um IP fixo para o pod. Mesmo que morra, referência de IP continua fixa. Também serve como load balancer, caso o pod em um nó morra. 
  - Serviço pode ser interno (para databases privados, por ex.) ou externos (para apps que devem ser acessados através da internet pública)
  - URL do serviço recebe IP do nó e porta do serviço
  - Ingress é o recurso que surge como domain name system (DNS), pra mascarar o IP/porta do Nó
  - Deployments servem para dizer quantos pods vão ser criados e para efetivamente criá-los (réplicas e outras configs adicionais)
  - Configuração do pod pode ser passada diretamente no momento da sua criação, no comando create deployment ou através de um .yaml, com o comando apply arquivo.yaml
  - Databases são replicáveis através do StatefulSet. Ele garante a replicação e escalada dos pods de bancos de dados, garantindo a sincronização entre eles, pra que não haja inconsistências na escrita/leitura de dados
  - É mais fácil fazer o deploy do database por fora do K8
  - Três processos precisam ser instalados em cada nó:
    - kubelete: responsável por iniciar o pod com um container específico
    - kubeproxy: garante que comunicação entre os pods e seus serviços seja inteligente/performático (por ex.: vai direcionar o tráfego de um pod com o app pra um pod com banco do mesmo nó e não pra um pod com banco de outro nó)
    - container runtime: responsável por executar e gerenciar containers

MASTER NODE - usa menos recursos que os worker nodes
  - possui quatro serviços principais
  - API SERVER: recebe requisições do cliente, seja por API, UI ou mesmo linha de comando (kubectl). Lida com autenticação. É o gateway do Kubernetes. Da informações sobre os nós por cliente
  - SCHEDULER: inicializa os pods e joga eles nos nós corretos, baseado no uso de recursos da aplicação que vai pro pod e nos recursos dos nós. Ele chama o kubelete.
  - CONTROLLER MANAGER: detecta muanças de estado nos pods e envia requisições pro scheduler, pra que ele reviva os pods, por exemplo
  - ETCD: cérebro do master node, responsável por armazenar informações sobre os nós/pods e repassá-las para os demais serviços do master (nao armazena infos dos apps de dentro dos pods, apenas metadados dos pods em si)
    
CENÁRIOS ONDE PRECISA-SE DE MAIS RECURSOS
  - basta adicionar os processos em novos nós (master ou worker) que eles já serão considerados e utilizados pelo ETCD e cia

CONFIG MAP AND SERVICE
  - Para que voce nao tenha que fazer o re-deploy de uma aplicação, porque a url de conexao com o banco mudou, por exemplo. Vc muda a variavel no config map direto.
  - Usuário e senha podem mudar. Colocar essas infos no config map, por escrito, pode ser perigoso. Por isso, pode usar o Secret, ao invés do config map. Secrets salva as informações em formato binário, codificadas.

VOLUMES
  - armazenamento de dados é efemero. Para lidar com isso, existem os volumes (podem ser locais ou externos)


KUBECTL MAIN COMMANDS - CLI DO K8 (prática pode ser com minikube, localmente)

install hyperhit and minikube
brew update
brew install hyperkit
brew install minikube
kubectl
minikube

create minikube cluster
minikube start --vm-driver=hyperkit
kubectl get nodes
minikube status
kubectl version

delete cluster and restart in debug mode
minikube delete
minikube start --vm-driver=hyperkit --v=7 --alsologtostderr
minikube status

kubectl commands
kubectl get nodes
kubectl get pod
kubectl get services
kubectl create deployment nginx-depl --image=nginx
kubectl get deployment
kubectl get replicaset
kubectl edit deployment nginx-depl

debugging
kubectl logs {pod-name}
kubectl exec -it {pod-name} -- bin/bash

create mongo deployment
kubectl create deployment mongo-depl --image=mongo
kubectl logs mongo-depl-{pod-name}
kubectl describe pod mongo-depl-{pod-name}

delete deployment
kubectl delete deployment mongo-depl
kubectl delete deployment nginx-depl

create or edit config file
vim nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl get pod
kubectl get deployment

delete with config
kubectl delete -f nginx-deployment.yaml
#Metrics
kubectl top The kubectl top command returns current CPU and memory usage for a cluster’s pods or nodes, or for a particular pod or node if specified.


YAML CONFIG FILE FOR PODS

