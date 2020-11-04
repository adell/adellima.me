---
title: "Removendo pacotes sem uso no Ubuntu e Ansible"
date: 2020-11-04T03:53:19-03:00
draft: false
featureImage: images/allpost/allPost-6.jpg
postImage: images/single-blog/feature-image.jpg
---
Manter o Ubuntu (ou qualquer outra distribuição Linux) com somente os pacotes
em uso é uma boa prática de segurança, afinal uma breha pode estar naquele
pacote que nem usamos mais e ainda consome recurso na hora de fazer uma
atualização.

Mas também existe aqueles arquivos que acabam ficando orfãos no sistema. Quem
nunca se deparou com a mensagemde que não existe mais espaço livre na partição
boot?

Felizmente o [Debian](https://www.debian.org) e seus derivados(não sei em outras
 distribuições) contam com apt para resolver este problema de forma mais segura:

 ``` shell

      $ sudo apt autoremove

 ```
 Este comando remove do sistema tudo o que não já não está em uso, como kernel,
 libs e etc.

 Mas o problema é quando temos um parque mais extenso, e entrar em cada maquina
 e executar manualmente a ação pode ser um tanto quanto tedioso e demorado.

 ### Fazendo de forma automatica.
 O Ansible pode ajudar muito em uma situação assim, já que pode se conectar a 
 uma lista de servidores e executar nestes tarefas de forma um tanto quanto 
 faceis de serem configuradas e aplicadas.

 #### O que é preciso?