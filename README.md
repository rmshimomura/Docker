# Docker

## Comandos básicos:
- ```docker ps``` : Lista os containers em execução
    - ```docker ps -a``` : Lista todos os containers rodados (junto com os que estão rodando no momento)
- ```docker run <imagem>``` : Cria um novo container
    - ```docker run -it <imagem>``` : Cria um novo container deixando ele interativo no terminal
    - ```docker run -d <imagem>``` : Cria um novo container deixando ele "detached", ou seja rodando no background, sem travar o terminal
    - ```docker run --name <nome> <imagem>``` : Cria um novo container com o nome passado depois da flag ```--name```
    - ```docker run --rm <imagem>``` : Quando o container parar, ele vai ser deletado
    - ```docker run -p <porta_do_host:porta_do_container> <imagem>``` : Cria um container e expõe a porta dele para o mundo afora, lembrando que tem que a ordem importa e tem que colocar os dois pontos entre os números das portas
- ```docker start <id>``` : "Liga" o container novamente
    - ```docker start -it <id>``` : "Liga" o container novamente, deixando ele iterativo
- ```docker rm <id>``` : Remove um container
    - ```docker rm -f <id>``` : Força a remoção de um container
- ```docker stop <id>``` : Para um container sem removê-lo
- ```docker image ls``` ou ```docker images``` : Lista as imagens guardadas em cache
    - ```docker image rm <id>``` ou ```docker rmi <id_imagem>```: Remove uma imagem específica
- ```docker logs <id>``` : Mostra o que aconteceu em um container específico
    - ```docker logs -f <id>``` : Parecido com o anterior, porém todas as operações que aconteceram no container vão estar aparecendo em tempo real no terminal
- ```docker build <diretório_da_imagem>``` : Construir uma imagem Docker a partir de um Dockerfile (necessário fazer antes de rodar a imagem de fato!)
    > Obs: Qualquer alteração em um arquivo que faz parte do Docker file, precisa dar build novamente!
    - ```docker build <diretório_da_imagem> -t <repo>:<tag>``` : Semelhante ao comando básico, com diferença é que o repositório e a tag vão ser criados durante o build da imagem do Dockerfile
- ```docker cp <fonte> <destino>``` : Copia algum arquivo de um container para um diretório
        > Exemplo: docker cp node_diferente_1:/app/app.js ./copy/
- ```docker pull <imagem>``` : Download de alguma imagem no hub
- ```docker tag <id> <repository>:<tag>``` : Coloca o repository e tag, e quando fizermos docker ps, aparecerá o que colocamos nos argumentos desse comando
    > Obs: Se a tag não for especificada, latest será a padrão
- ```docker top <id>``` : Mostra informações sobre o container específico rodando
- ```docker inspect <id>``` : Mostra propriedades do container, várias informações detalhadas
- ```docker stats``` : Mostra como os recursos estão sendo alocados no momento
- ```docker login``` : Login com os dados do https://hub.docker.com/
- ```docker logout``` : Logout do https://hub.docker.com/
- ```docker push <id_imagem>``` : Envia a imagem para um repositório no Docker hub
    > Obs: Tem que fazer build com a tag -t seguido de <username>/nome_repositorio
    > Para atualizar uma imagem, devemos fazer a build novamente, mas com outra tag
- ```docker system prune``` : Remove todos os containers, imagens e volumes que não estão sendo utilizados
## Volumes

Existem 3 tipos de volumes:
- Anônimos: Diretórios criados pela flag -v, com um nome aleatório - não conseguimos reaproveitamento aqui
- Nomeados: Volumes com nomes, podemos nos referir a estes facilmente e saber para que são utilizados no nosso ambiente
- Bind mounts: Um forma de salvar dados na nossa máquina, sem o gerencimanto do Docker, precisamos informar um diretório aqui

### Comandos básicos de volumes
- ```docker run -v <diretório>``` : Cria um volume anônimo no diretório passado
- ```docker run -v <nome_do_volume>:<diretório>``` : Cria um volume nomeado no diretório passado
    > Exemplo : ```docker run -d -p 80:80 --name phpmessages_container -v /mnt/c/Users/Usuario/Desktop/Docker/2_volumes/messages:/var/www/html/messages --rm phpmessages```

    > Obs : Se colocarmos com 'ro' no final, o volume vai ser somente leitura, exemplo: ```docker run -d -p 80:80 --name phpmessages_container -v /mnt/c/Users/Usuario/Desktop/Docker/2_volumes/messages:/var/www/html/messages:ro --rm phpmessages```

- ```docker volume ls``` : Mostra os volumes criados

- Para o Bind Mount, fazemos: 
    > ```docker run -d -p 80:80 --name phpmessages_container -v /mnt/c/Users/Usuario/Desktop/Docker/2_volumes/:/var/www/html/ --rm phpmessages```
    
    > Perceba que aqui nós tiramos o /messages do path do volume, isso fez com que o bind mount entrasse em ação e atualizasse nosso projeto em tempo real. E é assim que vamos fazer daqui para frente com os projetos.
- ```docker volume create <nome_do_volume>``` : Cria um volume nomeado
- ```docker volume inspect <nome_do_volume>``` : Mostra informações sobre o volume
- ```docker volume rm <nome_do_volume>``` : Remove um volume nomeado
- ```docker volume prune``` : Remove todos os volumes que não estão sendo utilizados por nenhum container

### Comandos básicos de rede/network
- ```docker network create <nome_da_rede>``` : Cria uma rede (tipo bridge, conectando o container com outros)
- ```docker network create -d <driver> <nome_da_rede>``` : Cria uma rede com um driver específico
- ```docker network rm <nome_da_rede>``` : Remove uma rede
- ```docker network prune``` : Remove todas as redes que não estão sendo utilizadas por nenhum container
- ```docker network connect <nome_da_rede> <nome_do_container>``` : Conecta um container a uma rede
- ```docker network disconnect <nome_da_rede> <nome_do_container>``` : Desconecta um container de uma rede
- ```docker network ls``` : Lista as redes criadas
- ```docker network inspect <nome_da_rede>``` : Mostra informações sobre a rede

## Compose

Usamos um arquivo chamado docker-compose.yaml para criar o nosso compose, e nele colocamos as configurações de cada container que queremos criar, e depois rodamos o comando ```docker-compose up``` para subir todos os containers de uma vez só.

### Comandos básicos de compose

- ```docker-compose up``` : Cria todos os containers que estão no arquivo docker-compose.yaml
    - ```docker-compose up -d``` : Cria todos os containers que estão no arquivo docker-compose.yaml em background

- ```docker-compose down -v``` : Para todos os containers e remove os volumes que foram criados
    - ```docker-compose down``` : Para todos os containers e remove os volumes que foram criados
    - ```docker-compose down --rmi all``` : Para todos os containers e remove os volumes que foram criados, e remove as imagens que foram criadas
    - ```docker-compose down --rmi all -v``` : Para todos os containers e remove os volumes que foram criados, e remove as imagens que foram criadas, e remove as redes que foram criadas

- ```docker-compose ps``` : Mostra os containers que estão rodando que foram criados pelo compose

## Swarm

### Comandos básicos de swarm

- ```docker swarm init``` : Inicia o swarm
    - ```docker swarm init --advertise-addr <ip>``` : Inicia o swarm com um ip específico
- ```docker swarm leave``` : Sai do swarm
    - ```docker swarm leave -f``` : Sai do swarm forçadamente
- ```docker swarm join --token <token> <ip>:<porta>``` : Faz um nó se juntar ao swarm
- ```docker swarm join-token manager``` : Mostra o token para um nó se juntar como manager (funciona apenas quando o nó é manager)
- ```docker node ls``` : Lista os nós do swarm
- ```docker node rm <id>``` : Remove um nó do swarm 
- ```docker node update --availability drain <id>``` : Coloca um nó em modo de manutenção, fazendo com que ele não receba mais containers
- ```docker node ps ``` : Mostra os containers que estão rodando em um nó
- ```docker service create --name <nome> --replicas <numero> <imagem>``` : Cria um serviço com um número definido de replicações 
- ```docker service ls``` : Lista os serviços do swarm
- ```docker service rm <nome_do_serviço>``` : Remove um serviço do swarm
- ```docker service ps <nome_do_serviço>``` : Mostra os containers que estão rodando do serviço
- ```docker service scale <nome>=<replicas>``` : Escala um serviço
- ```docker service update --image <imagem> <nome_do_serviço>``` : Atualiza a imagem de um serviço
    #### Importante saber sobre redes em swarm
    1. A conexão entre intâncias usa um driver diferente, chamado de overlay network
    2. Primeiro cria-se a rede com o comando ```docker network create -d overlay <nome_da_rede>```
    3. Depois, quando for criar o serviço, usa-se o comando ```docker service create --name <nome> --replicas <numero> --network <nome_da_rede> <imagem>```
- ```docker service update --network-add <nome_da_rede> <nome_do_serviço>``` : Adiciona uma rede a um serviço
- ```docker info``` : Mostra informações sobre o swarm
- ```docker stack deploy -c <arquivo> <nome>``` : Cria um stack com base em um arquivo de configuração

# Kubernetes

## Minikube

### Comandos básicos de minikube

- ```minikube start``` : Inicia o minikube
    - ```minikube start --driver=<driver>``` : Inicia o minikube com um driver específico
>> Observação : Sempre que o computador for reiniciado, o minikube para, então é necessário iniciar ele novamente
- ```minikube stop``` : Para o minikube
- ```minikube status``` : Mostra o status do minikube
- ```minikube dashboard``` : Abre o dashboard do minikube
    - ```minikube dashboard --url``` : Mostra a url do dashboard do minikube
- ```minikube service <nome_do_serviço>``` : Abre o serviço no navegador

### Comandos básicos de kubectl

- ```kubectl create deployment <nome> --image=<imagem>``` : Cria um deployment
- ```kubectl get deployments``` : Lista os deployments
- ```kubectl describe deployments``` : Mostra informações sobre os deployments
    - ```kubectl describe deployment <nome>``` : Mostra informações sobre um deployment específico
- ```kubectl get pods``` : Lista os pods
- ```kubectl describe pods``` : Mostra informações sobre os pods
    - ```kubectl describe pod <nome>``` : Mostra informações sobre um pod específico
- ```kubectl config view``` : Mostra informações sobre o cluster
- ```kubectl expose deployment <nome> --type=<tipo> --port=<porta>``` : Cria um serviço
- ```kubectl scale deployment/<nome> --replicas=<numero>``` : Escala um deployment (aumentar ou diminuir o número de pods)
- ```kubectl get rs``` : Lista os replicasets
- ```kubectl get services``` : Lista os serviços
- ```kubectl describe services``` : Mostra informações sobre os serviços
- ```kubectl set image deployment/<nome> <nome_container>=<nova_imagem>``` : Atualiza a imagem de um deployment
- ```kubectl rollout status deployment/<nome>``` : Mostra o status de uma atualização de imagem
- ```kubectl rollout undo deployment/<nome>``` : Desfaz uma atualização de imagem
- ```kubectl delete service <nome>``` : Deleta um serviço
    >> Observação: deletar um service não para o deployment
- ```kubectl delete deployment <nome>``` : Deleta um deployment
- ```kubectl apply -f <arquivo>``` : Cria um deployment ou service com base em um arquivo de configuração
- ```kubectl delete -f <arquivo>``` : Deleta um deployment ou service com base em um arquivo de configuração

#### Serviços

As aplicações do Kubernetes não tem conexão com o mundo externo, então criamos um Service que possibilita expor os pods para o mundo externo.

>> Observação, pod vs containers vs deployments:
```
    Um container é a unidade em que a aplicação é empacotada junto com suas dependências e bibliotecas. É dentro do container que a aplicação em si é executada.

    Um pod é uma abstração de nível superior que engloba um ou mais containers relacionados que são co-localizados e compartilham o mesmo ambiente de execução. Um pod é a menor unidade que pode ser implantada, escalada, gerenciada e programada no Kubernetes. Todos os containers em um pod compartilham o mesmo endereço IP, namespace de rede e volumes de armazenamento.

    Um deployment é um objeto no Kubernetes usado para gerenciar e controlar a criação e atualização de um conjunto de pods. O deployment define a especificação para os pods, como a imagem do container, a quantidade de réplicas desejadas e as estratégias de implantação. Quando você cria um deployment, o Kubernetes cria e gerencia automaticamente os pods de acordo com a especificação definida no deployment. O deployment garante que o número de réplicas definido seja mantido, e se houver falhas ou atualizações, ele cuidará do processo de criação ou substituição dos pods conforme necessário.
```
>> TL;DR:
    >> - Container : É onde a aplicação roda
    >> - Pod : É a menor unidade do Kubernetes, é onde os containers rodam
    >> - Deployment : É um conjunto de pods

#### Modo delcarativo do Kubernetes

Utilizamos o modo declarativo para criar objetos no Kubernetes, ou seja, criamos um arquivo de configuração e passamos para o Kubernetes, que vai criar o objeto com base nesse arquivo.

As chaves mais utilizadas no arquivo de configuração são:
- apiVersion : Versão da API do Kubernetes
- kind : Tipo do objeto que será criado (Deployment, Service)
- metadata : Metadados do objeto
- replicas : Número de réplicas do objeto (nodes, pods)
- containers : Containers que serão criados (nome, imagem, portas, volumes)