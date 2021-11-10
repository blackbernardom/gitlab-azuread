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