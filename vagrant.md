# Vagrant

Será preciso instalar o [VirtualBox](https://www.virtualbox.org/) e o [Vagrant](https://www.vagrantup.com/)

## Preparação 

Alguns alunos relatam problemas de certificado nos repositórios do Vagrant. Para evitar problemas, baixe a imagem pré-configurada da máquina manualmente:

```bash
vagrant box add --insecure debian/buster64 --provider virtualbox
```

Baixar os arquivos deste repositório clonando-os através do [git](https://git-scm.com/) ou fazendo download do .zip em https://github.com/4linux/ansible-basico/archive/refs/heads/master.zip.

Entrar no diretório e executar através do terminal:

```bash
vagrant up --provider virtualbox
```

> Este passo levará algum tempo.

As máquinas serão criadas no VirtualBox, ao término será possível listá-las com:

```bash
vagrant status
```

Para acessar qualquer uma das máquinas, digite `vagrant ssh` seguido de seu nome:

```bash
vagrant ssh m1
```

Pronto, você está no terminal da máquina!

## Comandos Básicos

A etapa de criação e provisionamento acontece apenas uma vez, chamadas subsequentes a `vagrant up` apenas iniciarão as máquinas. Todos os comandos devem ser executados no diretório em que o arquivo `Vagrantfile` está.

Para verificar as maquinas:

```bash
vagrant status
# Current machine states:
# 
# m1                        running (virtualbox)
# m2                        running (virtualbox)
# m3                        running (virtualbox)
# 
# This environment represents multiple VMs. The VMs are all listed
# above with their current state. For more information about a specific
# VM, run `vagrant status NAME`.
```

Para parar as máquinas:

```bash
vagrant halt
```

Para reinicar as máquinas:

```bash
vagrant reload
```

Para destruir o ambiente:

```bash
vagrant destroy
```
