# Conceitos para administrar uma provedora de cloud
Aqui irei inserir o que se faz necessário para se tornar um administrador de provedoras de cloud.


### Kubernetes 

Após você aprender separadamente sobre alguma provedora de cloud, vamos ao próximo passo, aprender a gerenciar uma infraestrutura em cloud.  
  
Uma das principais partes do gerenciamento pode ser o Kubernetes, que nada mais é que um orquestrador de contêiner.

Um contêiner é um computador (parte logica) que funciona de modo isolado, tendo dentro de cada contêiner:

*repositórios.  
*bibliotecas.  
*usuários.  
*senhas e arquivos.  
*, etc.  

Tudo isso funcionando de forma isolada dentro de cada contêiner sem um invadir o conteúdo do outro (se necessário é possível).  
Dito isso podemos afirmar que cada contêiner é como se fosse uma máquina virtual, que diferente dessas não compartilham dos mesmos repositórios que o computador possui,
até porque o contêiner como já informado funciona de forma isolada.

### contêiner engine

Seguindo com as informações sobre Kubernetes vamos entender o que é um contêiner engine.  
O contêiner eigine é o responsável por criar o contêiner, ver se o contêiner esta saudável, ele também cria ponto de montagem e insere a rede,
ou seja, quando falamos de volume ou algum diretório específico estamos falando do contêiner eigine.

Mesmo sendo tão importante lembre-se o contêiner eigine apenas trata dessas coisas ditas a cima, quando falamos de kernel somente o contêiner runtime consegue se comunicar com ele.  

### contêiner runtime

O contêiner runtime e o responsavel por se comunicar com o kernel, ele serve para executar os contêineres.
A Função dele é garantir que o contêiner dele esta em execução, ele garante a construção do contêiner e o isolamento.

O contêiner runtime tem três tipos, high level, low level e sandbox.

A low level tem uma comunicação direta com o kernel, já o high level não tem contato com o kernel ele e executado por um conteiner engine. 

### OCI - Open contêiner Iniciative 

O OCI foi o criador do conceito de contêiner e ele fez isso a parir de uma iniciativa de software livre com o intuito de fazer com que todos os contêineres criados se conversem.
No caso, a ideia era criar uma normalidade para os contêineres, obrigando a todos os que vierem depois a seguir um mesmo caminho.  

Kubernetes foi desenvolvido pelo google em 2014, não era aberto ao publico e tinha o nome de boorg, por isso foi desenvolvido em GO.

### Arquitetura do Kubernetes

*Computadores são chamados de NODES.  
*Temos os computadores são chamados de control plane e workes.   
*Control plane controla o clusters, é responsabilidade dele de controlar a saude e o estado do clusters.  
*Os Workes são os responsaveis em carregar as aplições, são os nodes que rodam as aplicações.  
*É possivel carregar as aplicações nos control planes porém não é o recomendado.  

### Componentes do control planes

Dentro do control planes temos muitos componentes, um dos mais importantes é o ETCD, ele é o banco do cluster, 
não é banco de dados, porém ele guarda todo o estado no cluster.

Ele é considerado o cerebro de todo o cluster e caso você mesmo esteja gerenciando o cluster saiba que é necessario ter diversas redundancias dessa aaplição.

Cada nó do control plane pode ter ou não um ETCD.

O ETCD guarda as informações que o Kube APIserver passa para ele, o Kube é quem conversa com todos os componentes e armazena as informações no ETCD,
nesse caso o ETCD seria a parte do cerebro que armazena. Já a parte pensante, a que processa dados, seria o Kube APIserver, toda a comunicação do cluster passa por ele.

Lembrando que o ETCD conversa apenas com o Kube APIserver que por sua vez conversa com todo o control plane.

já a proxima parte do control plane é o Kube Scheduler, ele assim como o nome diz é de agendamento, ele é responsavel pelo gerenciamento/agendamento e por definir aonde vai os conteiners.

Seguimos agora com o Kube controle manager, esse é o controlador de todos as partes dentro do control plane, o controlador do cluster.
Dentro do kubernets tem varios controladores, um para cada parte.

### Conponentes dos workes

O primeiro componente que temos no funcionamento dos workes é o Kubelet, ele é como se fosse o agente, ele fica dentro de todos os nós que temos nos workes.
Ele é quem sempre vai estar vendo se esta tudo OK com os conteiners e se não precisa fazer algo a mais por eles, ele entra especificamente com o APIserver.

O proximo componente assim como o kubelet também esta em todos os nós/nodes, esse é o kube proxy, ele que faz a comunicação dos nodes com os resto do mundo, ele que faz o conteiner se comunicar pela internet.

### Portas TCP e UDP

Abaixo esteremos indicando cada porta que o kubernetes precisa para estar funcionando perfeitamente.

*APIserver = 6443 - TCP   
*ETCD = 2379 - 2380 - TCP  
*Kube Scheduler = 10251 - TCP  
*Kubelet = 10250 - TCP  
*Kube controler = 10252 - TCP   

O unico que não tem uma porta propria é o Kube proxy, porém tem um outro componente chamado Nodeport, esse é o encarregado em conectar seu serviço que pode estar usando qualquer porta com uma porta do nó.

Porém essa porta sempre vai ser uma "alta", começando pela 30000 até a 32767 (TCP), caso você va usar o node porte como serviço precisa habilitar também as portas descritas anteriormente.
Agora se for utilizar apenas os componentes padrão, será necessario apenas as portas na tabela acima, pois essas são as comuns.

Outro componente que também utiliza uma porta especifica é wave net, esse utiliza a porta 6783 (TCP e UDP), o que esse componente faz nos explicaremos mais para frente.

### Introduçao ao pods, replica sets, deployments e service.

Quando falamos de Kubernetes, o menor componente de gerenciamento não é o conteiner é são os pods.
Um pod, ele contem um conteiner porém não só um como varios, se um ou mais conteiner estão dentro do mesmo pode, eles compartilhão as mesmas informações/comfigurações.

Um pode tem somente um IP, mesmo que tenha mais de um conteiners.

Se eu quiser ter mais de um pod que funcione da mesma forma, basta copiar um pod e colar quantas vezes for necessario.

Os conteiners ficam dentro do pod, os pods dentro dos nodes (nós), e se eu quiser gerenciar tudo isso eu utilizo o deployment.

O Deployments por sua vez tem um nome, como por exemplo, giropops.

Já o replica sets seria o responsavel pelas replicas, pelos nodes, e sempre que cair um node quem vai criar um novo para pleno funcionamento e o replica sets.
O replica sets e criado pelo deployments, e quando  eu mudo algo no deployment ele mata o replica sets antigo e cria um novo com as configurações atualizadas.

Todos os componentes acima estão dentro do que nos chamamos de sercice, existem varios tipos de service, o do exemplo é o service node port, esse é responsavel pelo acesso aos podes pelo lado de fora do nó.

Agora quando eu quiser acessar um node, especificamente um conteiner dentro dele, eu preciso me conectar na porta "alta" que por exemplo pode ser a 30001.
Essa que foi atrelada atravez do node port a porta que o componente usa para trabahar, como por exemplo a porta 80.
É dessa forma que acessamos os conteiners, que de forma escrita ficaria http://node:30001.


