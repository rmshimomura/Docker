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
