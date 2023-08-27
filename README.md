# Conceitos para administrar uma provedora de cloud
Aqui irei inserir o que se faz necessário para se tornar um administrador de provedoras de cloud.


### Kubernetes 

Após você aprender separadamente sobre alguma provedora de cloud, vamos ao próximo passo, aprender a gerenciar uma infraestrutura em cloud.  
  
Uma das principais partes do gerenciamento pode ser o kubernetes, que nada mais é que um orquestrador de contêiner.

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

Seguindo com as informações sobre kubernetes vamos entender o que é um contêiner engine.  
O contêiner eigine é o responsavel por criar o contêiner, ver se o contêiner esta saudavél, ele também cria ponto de montage e insere a rede,
ou seja quando falamos de volume ou algum diretorio especifico estamos falando do contêiner eigine.

Mesmo sendo tão importante lembre-se o contêiner eigine apenas trata dessas coisas ditas a cima, quando falamos de kernel somente o contêiner runtime consegue se comunicar com ele.  




