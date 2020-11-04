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

 Para seguir os passos a seguir, alguns requesitos são necessarios:
 * Ter o [Ansible instalado](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) em sua maquina.
 * Ter uma pasta com a seguinte arvore de arquivos.

```
├── main.yml
├── hosts
└── roles
    └── autoremove
        └── tasks
            └── main.yml
```
Esta é a forma que gosto de usar, existem outras, mas não serão tratadas aqui.

main.yaml
``` yaml
---
# Diz quais os host do inventario serão afetados
- hosts: all
  # Ativa a escalada de privilegio.
  become: true

  # Não usaremos variaveis da maquina destino, como por exemplo da versão
  # do ubuntu, então podemos desativar a coleta de informações,
  # fica mais rapido
  gather_facts: no

  # Qual a role será usada para efetuar a tarefa
  roles:
    - 'autoremove'
 ```

 hosts
``` yaml
# Nome do grupo de servidores
[all]
web	        ansible_host=10.0.49.102 ansible_sudo_pass='MinhaSenha' ansible_ssh_pass='MinhaSenha'
mysql       ansible_host=10.0.49.103 ansible_sudo_pass='MinhaSenha' ansible_ssh_pass='MinhaSenha'
 ```

 main.yaml
``` yaml
---
# Texto descritivo que aparece antes de executar a tarefa
- name: Clean unwanted olderstuff
  # Nome do modula usado
  apt:
    # Ação a ser tomada
    autoremove: yes
 ```

 Com todos os arquivos em seus devidos lugares, basta um comando e todos os 
 servidores da lista (hosts) sofrerão as ação desejada.

```shell

  $ ansible-playbook main.yml -i hosts

```