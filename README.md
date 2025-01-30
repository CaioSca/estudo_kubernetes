# Estudo Kubernetes

INFRA BÁSICA

  - Pod é a unidade mínima do kubernetes. Ele é montado como uma camada de abstração sobre o container
  - Pods tem IPs únicos, ou seja, quando pod morre, é criado um novo, com novo IP
  - Serviço existe para definir um IP fixo para o pod. Mesmo que morra, referência de IP continua fixa. Também serve como load balancer, caso o serviço em um nó morra. 
  - Serviço pode ser interno (para databases privados, por ex.) ou externos (para apps que devem ser acessados através da internet pública)
  - URL do serviço recebe IP do nó e porta do serviço
  - Ingress é o recurso que surge como domain name system (DNS), pra mascarar o IP/porta do Nó
  - Deployments servem para dizer quantos pods vão ser criados (réplicas e outras configs adicionais)
  - Databases são replicáveis através do StatefulSet. Ele garante a replicação e escalada dos pods de bancos de dados, garantindo a sincronização entre eles, pra que não haja inconsistências na escrita/leitura de dados
  - É mais fácil fazer o deploy do database por fora do K8
  - Três processos precisam ser instalados em cada nó:
    - kubelete: responsável por iniciar o pod com um container específico
    - kubeproxy: garante que comunicação entre os pods e seus serviços seja inteligente/performático (por ex.: vai direcionar o tráfego de um pod com o app pra um pod com banco do mesmo nó e não pra um pod com banco de outro nó)
    - container runtime: responsável por executar e gerenciar containers

MASTER NODE
  - possui quatro serviços principais
  - API SERVER: recebe requisições do cliente, seja por API, UI ou mesmo linha de comando (kubectl). Lida com autenticação. É o gateway do Kubernetes. Da informações sobre os nós por cliente
  - SCHEDULER: inicializa os pods e joga eles nos nós corretos, baseado no uso de recursos da aplicação que vai pro pod e nos recursos dos nós. Ele chama o kubelete.
  - CONTROLLER MANAGER: detecta muanças de estado nos pods e envia requisições pro scheduler, pra que ele reviva os pods, por exemplo
  - ETCD: cérebro do master node, responsável por armazenar informações sobre os nós/pods e repassá-las para os demais serviços do master (nao armazena infos dos apps de dentro dos pods, apenas metadados dos pods em si)

CONFIG MAP AND SERVICE
  - Para que voce nao tenha que fazer o re-deploy de uma aplicação, porque a url de conexao com o banco mudou, por exemplo. Vc muda a variavel no config map direto.
  - Usuário e senha podem mudar. Colocar essas infos no config map, por escrito, pode ser perigoso. Por isso, pode usar o Secret, ao invés do config map. Secrets salva as informações em formato binário, codificadas.

VOLUMES
  - armazenamento de dados é efemero. Para lidar com isso, existem os volumes (podem ser locais ou externos)
    
