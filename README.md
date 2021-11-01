# Ansible

[Ansible](https://www.ansible.com/) é uma ferramenta de automação e gerência de configuração, sem a necessidade de um centralizador e de agentes instalados nas máquinas.
Podemos configurar centenas de máquinas de um ponto qualquer através de um arquivo de formato muito simples chamado [YAML](https://pt.wikipedia.org/wiki/YAML).

Para preparar o ambiente seguir as instruções em [virtualbox.md](https://github.com/4linux/ansible-basico/blob/master/virtualbox.md) ou [vagrant.md](https://github.com/4linux/ansible-basico/blob/master/vagrant.md).

Recomendamos o uso do **vagrant** pois é uma ferramenta que nos auxilia a criar ambientes de teste da maneira mais cômoda possível, como segunda opção fornecemos a **ova** que poderá estar indisponível após o evento.

## Inventário

O inventário é o arquivo em que definimos as nossas máquinas, pode ter vários formatos como INI, JSON ou YAML.
Dentro do inventário podemos criar estruturas complexas de grupos e variáveis como também podemos utilizar um formato de uma simples lista.

Um inventário básico com variáveis globais e de grupo no formato `.ini` pode ser criado da seguinte maneira:

```
[all:vars]
ansible_python_interpreter=/usr/bin/python3

[balancer]
172.27.11.10

[webserver]
172.27.11.20
172.27.11.30

[webserver:vars]
versao_apache=2.4
```

Cada par de `[]` forma um grupo, os grupos com sufixo `:vars` indicam variáveis válidas somente para aquelas máquinas do grupo. Uma exceção é o `[all:vars]` que define variáveis para todas as máquinas do inventário.

Estas variáveis podem ser utilizadas durante as tarefas do Ansible, algumas podem já existir, no caso de `ansible_python_interpreter` e outras podem ser criadas por nós como `versao_apache`.

### Autenticação

O Ansible se comunica com as máquinas "Unix Like" através de [SSH](https://pt.wikipedia.org/wiki/Secure_Shell) e nas máquinas Windows através de [WinRM](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html).

A autenticação pode ser feita por usuário e senha ou através de chaves criptográficas.

## Comandos Avulsos

Com um inventário podemos executar comandos simples nas máquinas, para fazer verificações, instalar pacotes, etc. A estes comandos damos o nome de **ad-hoc**.

Os comandos nada mais são do que "módulos", invocamos módulos que fazem determinadas coisas nas máquinas, tudo no Ansible gira em torno dos módulos.

Durante a execução destes comandos podemos informar o usuário, a senha ou a chave de conexão:

```bash
ansible --inventory inventario.ini all --user root --private-key chave --module ping
```

> Neste caso especificamos o inventário, usuário, senha e o módulo [ping](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html).

A única aparição sem nome é a palavra chave `all` que indica todos os presentes no inventário, mas pode ser o nome de um grupo ou mesmo o endereço de uma máquina em específico.

Quando o módulo possuir argumentos podemos especificá-los, como a instalação de um pacote pelo módulo [package](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html):

```bash
ansible -i inventario.ini all -u root --private-key chave -m package -a 'name=htop state=present'
```

> Abreviamos os parâmetros, veja que passamos os argumentos **name** e **state** ao módulo **package**.

## Playbooks

As playbooks são formas de executar uma série de comandos do Ansible em uma única execução em um número qualquer de máquinas. Através das playbooks também podemos controlar o fluxo e utilizar lógica para repetir ou pular tarefas.

As playbooks são escritas através de YAML e utilizam-se dos módulos existentes e de palavras chaves para ações específicas como `loop` para repetições ou `when` para condições.

### YAML

No formato YAML devemos utilizar **dois pontos** para separar a `chave` do `valor`, **espaços** para identação e hierarquia e **traços** para indicar itens de uma lista:

```yml
nome: Ansible Básico
duracao: 1 horas
temas:
- O que é ansible
- Inventário
- Comandos Avulsos
- Playbooks
requisitos:
  computador:
  - Linux
  - Windows
  - MacOS
  conhecimentos:
    linux: Apenas o básico
    vms: Criação de máquinas no virtualbox
```

Podemos dizer que o YAML todo é um objeto ou um dicionário.

Na lista acima:

- `temas` é uma lista;
- `requisitos` é um objeto/dicionário, possui duas chaves filhas, `computador` e `conhecimentos`;
- `requisitos.computador` é uma lista;
- `requisitos.conhecimentos` é um objeto/dicionário, possui duas chaves filhas, `linux` e `vms`.

### Exemplo

Abaixo temos uma pequena playbook que, como root graças ao `become`, instala o apache e copia o conteúdo de um site para dentros da máquinas:

```yml
- hosts: all
  become: yes
  tasks:
  - name: Garantindo presença do Apache e seus módulos
    package:
      name: ['apache2', 'libapache2-mod-security2']
      state: present
  - name: Garantindo site
    copy:
      src: site/
      dest: /var/www/html
```

A playbook tem um formato fixo, composto de algumas configurações e sua lista de tarefas `tasks`. É possível repetir estas configurações e suas `tasks` indefinidamente na playbook, indicando outros grupos de `host` e outras configurações.

Para executar esta playbook, bastaria digitar:

```bash
ansible-playbook -i inventario.ini -u root --private-key chave playbook.yml
```
