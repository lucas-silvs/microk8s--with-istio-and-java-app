# microk8s--with-istio-and-java-app

Repositório de estudo da ferramenta MicroK8s, que é uma ferramenta para implantação de ambientes Kubernetes em segundos. Para o estudo, será utilizado o Lens como ferramenta de monitoração, uma aplicação Spring Boot 2.7.5 com Java 11 para ser orquestrada pelo cluster e um banco de dados mysql

##Execução Local
Para execução local, deve primeiro iniciar o microk8s com o comando abaixo:

```
microk8s start
```

para analisar o status da inicialização do microk8s, execute o seguinte comando:

```
microk8s status
```

Após a inicialização completa do microK8s, devemos criar o container da aplicação Java. Dentro do diretorio da aplicação Java, execute o seguinte comando:

```
docker build -t nome_da_imagem_desejada .
```

Com isso será criado uma imagem local da aplicação Java para ser executada em um container Docker. Para executar o container localmente (fora do Kubernetes) execute o comando abaixo:

```
docker run --name nome_do_container_desejado -e SPRING_PROFILES_ACTIVE=docker -p 5000:5000 -i nome_da_imagem_desejada
```

No navegador, acesse a url https://localhost:5000/hello-world , o esperado é aparecer a seguinte mensagem:

```
 Hello World - aplicação funcionando dentro do docker e orquestrado pelo Kubernetes
```

Após a confirmação da execução correta do container, é necessário adicionar a imagem para ser utilizada pelo cluster criado pelo microK8s. primeiro devemos salvar a imagem em um arquivo TAR:

```
docker save nome_imagem_desejada > nome_imagem_desejada.tar
```

E em seguida adiciona-lá ao microK8s:

```
microk8s ctr image import nome_imagem_desejada.tar
```

Para confirmar se a imagem foi adicionada corretamente, liste as imagens que estão sendo utilizadas pelo microK8s:

```
microk8s ctr images ls
```

##Referencias:
[Orientações MicroK8s](https://www.gasparbarancelli.com/post/instalando-microk8s-no-Ubuntu?lang=pt)
[MicroK8s](https://microk8s.io/)
[Artefatos Kubernetes](https://kubernetes.io/docs/home/)
[Docker](https://docs.docker.com/)
