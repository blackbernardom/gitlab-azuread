## Preparação RDS
 - lembrando sempre de olhar as definições de segurança da aws para o acesso correto.
 - criar um RDS com engenharia postgresql. Criar ele com user de nome GITLAB.
 - dentro dele executar o seguinte comando:
  
 `CREATE DATABASE gitlabhq_production OWNER gitlab TABLESPACE default;` 

------------------------------------------------------------------------------

## Azure 
 - criar um aplicativo com o nome do projeto "gitlab"
 - criar a secret de autenticação dele
 - adicionar a url do app com o seguinte caminho ; "https://myapply/users/auth/azure_oauth2/callback"
 - na API permissions add: email, openid, profile
 
 ------------------------------------------------------------------------------

## GitlabRunner
 - install helm -  https://docs.gitlab.com/runner/install/kubernetes.html#configuring-gitlab-runner-using-the-helm-chart
 - criar o arquivo values.yaml - https://gitlab.com/gitlab-org/charts/gitlab-runner/blob/master/values.yaml

---------------------------------------------------------------------------------
 ## comandos no kubernetes
  `kubectl get namespace`
  
  `kubectl  create namespace "nomenamespace"`
  
  `kubectl create -f . " file.yaml"`
  
  `kubectl get po -n nomenamespace "pegando os pods"`
  
  `kubectl get sts -n namenamespace "pegando o statefulset"`
  
  `kubectl get svc -n namenamespace "pegando os servicos"`
  
  `kubectl get pvc -n namenamespace "verificando nosso volume"`


----------------------------------------------------------------------------------

## Comandos no cluster Kubernetes do gitlab
 - existe uma pasta com os campos que devem ser adicionados no gitlab.rb, '/etc/gitlab/gitlab.rb 'basta copiar e colar no fim do arquivo, mas antes é necessario preencher com os dados da AZURE para a integração de AUTENTICAÇAO e os da AWS para CONEXÃO NO POSTGRESQL. 
 - apos alterar basta dar os seguintes comandos
  
  `gitlab-clt reconfigure`
  `gitlab-ctl restart`
  
  
  # GITLAB
 - Este projeto tem o ibjetivo de criarmos o gitlab no kubernetes.
 - Este exemplo está baseado em uma estrutura com EKS na aws. Seu deploy do git vai consistir em subir ele no eks com aws-controller.

## Preparação RDS
 - lembrando sempre de olhar as definições de segurança da aws para o acesso correto.
 - criar um RDS com engenharia postgresql. Criar ele com user de nome GITLAB.
 - dentro dele executar o seguinte comando:
  
 `CREATE DATABASE gitlabhq_production OWNER gitlab TABLESPACE default;` 
------------------------------------------------------------------------------

## Azure 
 - criar um aplicativo com o nome do projeto "gitlab"
 - criar a secret de autenticação dele
 - adicionar a url do app com o seguinte caminho ; "https://myapply/users/auth/azure_oauth2/callback"
 - na API permissions add: email, openid, profile
 
 ------------------------------------------------------------------------------

 # Como fazer
  
  * Para essa tarefa, você vai precisar de:
   - VS code
   - kubectl configurado com o ambiente em questão
   - helm3 instalado
   - ter acesso a aws console para verificar as regras de security group para ajustar a porta 22.

  * Ajustar os arquivos yamls para sua devida necessidade.
  * Posteriormente você pode entrar na pasta deploy-gitlab e usar o seguinte comando:
  
  
  `kubectl create -f .`
  
  
  * Posteriormente podemos verificar se está tudo certo com o pod, service e virtual service usando o describe.
  
  `kubectl describe "recurso" -n gitlab "nome-recurso"`

  * Após, tudo funcionando, podemos subir o gitlab-runner via helm, dentro da pasta gitlab-runner, baseado no arquivo values.yaml

   
   * Adicionando Repositório.
    
   ` helm repo add gitlab https://charts.gitlab.io `

   * Instalando runner   
   ` helm install --namespace gitlab gitlab-runner -f values.yaml gitlab/gitlab-runner `

   * Caso tenha que fazer alguma atualização, você pode usar esse comando também: 
   
` helm upgrade --namespace gitlab gitlab-runner -f values.yaml gitlab/gitlab-runner `
   - Importante, caso haja algum erro na hora de rodar o runner, é importante verificar, um dos possiveis erros nós conseguimos resolver com as seguintes resoluções:
    

  * RBAC: Por ser intra cluster, o serviço necessita de algumas permissões para um determinado usuário, você pode ver o erro que ele vai te retornar, entrar na pasta RBAC alterar o arquivo yaml que já tem as permissões exigidas e ajustar o usuário do erro que provavelmente ele vai pedir.
  * Cronjob: Outro possivel erro é na cron para rodar o runner, para resolver basta entrar na pasta, após analisar bem o erro para ter certeza que é esse o problema e usar o nosso comando favorito.
     ` kubect create -f .`

## Comandos no cluster Kubernetes do gitlab
 - existe uma pasta com os campos que devem ser adicionados no gitlab.rb, '/etc/gitlab/gitlab.rb 'basta copiar e colar no fim do arquivo, mas antes é necessario preencher com os dados da AZURE para a integração de AUTENTICAÇAO e os da AWS para CONEXÃO NO POSTGRESQL. 
 - apos alterar basta dar os seguintes comandos
  
  `gitlab-clt reconfigure`
  `gitlab-ctl restart`



# AWS
- Existe uma pasta aws com os recursos de network load balancer e target group, para fazer o git funcionar com um load balancer caso a gente não use o istio e o aws-controller.
