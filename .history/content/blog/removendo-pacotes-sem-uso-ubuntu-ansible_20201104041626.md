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