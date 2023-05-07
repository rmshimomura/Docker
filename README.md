# Docker

### Comandos básicos:
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