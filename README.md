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

## Deploy no Cluster Kubernetes utilizando MicroK8s

Após a confirmação da execução correta do container, é necessário adicionar a imagem para ser utilizada pelo cluster criado pelo microK8s. Primeiro devemos ativar o registry, uma addons do microK8s com o proposito de armazenar imagens docker geradas localmente para utilização dentro do cluster:

```
microk8s enable registry
```

Apos ativar o registry, devemos criar um build para ser enviado ao registry:

```
docker build . -t localhost:32000/nome_imagem_desejada:local
```

Em seguida devemos pegar o IMAGE ID da imagem gerada:

```
docker images
```

Com o ID da imagem, devemos criar uma tag da imagem:

```
docker tag image_id_da_imagem localhost:32000/nome_imagem_desejada:local
```

Com a imagem devidamente tageada, devemos envia-lá ao registry:

```
docker push localhost:32000/nome_imagem_desejada:local
```

Com a imagem salva no registry do cluster, agora devemos criar os artefatos do Kubernetes para a aplicação teste. Na pasta service--teste/kubernetes/local, você deve executar o seguinte comando abaixo:

```
microk8s kubectl apply -k .
```

Esse comando irá aplicar todos os artefatos Kubernetes listados no arquivo Kustomization.yaml

## Criação Ingress

Para poder realizar as requisições em um serviço hospedado em um cluster Kubernetes, precisamos expor o seu serviço. Existem muitas formas de expor o serviço no Kubernetes, mas aqui usaremos o ingress, que é um artefato do Istio para expor as aplicações, podendo ter host customizados, portas, e entre outras configurações. Para criar o ingress, entre na pasta Cluster Configuration e execute o seguinte comando abaixo:

```
microk8s kubectl apply -f ingress.yaml
```

## Referencias:

[Orientações MicroK8s](https://www.gasparbarancelli.com/post/instalando-microk8s-no-Ubuntu?lang=pt)

[MicroK8s](https://microk8s.io/)

[Artefatos Kubernetes](https://kubernetes.io/docs/home/)

[Kustomize](https://kustomize.io/)

[Docker](https://docs.docker.com/)
