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
