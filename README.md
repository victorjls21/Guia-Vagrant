# Guia-Vagrant
Guia prático de utilização do Vagrant desenvolvido para a disciplina de Infraestrutura como Código (IaC).
# Guia Prático de Vagrant

Este repositório apresenta um guia prático para instalação, configuração e utilização do Vagrant, desde a preparação do ambiente até a criação e o gerenciamento de máquinas virtuais.

## Sumário

1. [O que é Vagrant?](#1-o-que-é-vagrant)
2. [Como o Vagrant funciona?](#2-como-o-vagrant-funciona)
3. [Pré-requisitos](#3-pré-requisitos)
4. [Instalação](#4-instalação)
5. [Verificando a instalação](#5-verificando-a-instalação)
6. [Criando o primeiro projeto](#6-criando-o-primeiro-projeto)
7. [Entendendo o Vagrantfile](#7-entendendo-o-vagrantfile)
8. [Criando a máquina virtual](#8-criando-a-máquina-virtual)
9. [Acessando a máquina virtual](#9-acessando-a-máquina-virtual)
10. [Compartilhamento de arquivos](#10-compartilhamento-de-arquivos)
11. [Configuração de rede](#11-configuração-de-rede)
12. Provisionamento
13. Principais comandos
14. Encerrando e removendo a máquina
15. Exemplos práticos
16. Problemas comuns
17. Referências

---

## 1. O que é Vagrant?

O **Vagrant** é uma ferramenta de linha de comando desenvolvida pela HashiCorp para criar e gerenciar ambientes de desenvolvimento de forma padronizada e reproduzível.

Em vez de configurar manualmente uma máquina virtual sempre que um novo ambiente for necessário, o Vagrant permite definir as principais características desse ambiente em um arquivo chamado **Vagrantfile**.

A partir desse arquivo, é possível automatizar tarefas como:

- criar e iniciar máquinas virtuais;
- definir o sistema operacional utilizado;
- configurar memória e processadores;
- configurar redes e portas;
- compartilhar arquivos entre o computador e a máquina virtual;
- executar comandos e instalar softwares automaticamente;
- desligar, reiniciar ou remover o ambiente.

Dessa forma, diferentes integrantes de uma equipe podem utilizar o mesmo Vagrantfile para criar ambientes com configurações semelhantes em seus computadores.

### Por que utilizar o Vagrant?

Imagine que vários desenvolvedores precisam trabalhar em um mesmo projeto. Se cada integrante configurar manualmente seu ambiente, podem surgir diferenças de sistema operacional, versões de programas ou configurações.

Com o Vagrant, a configuração do ambiente pode ser definida em código e compartilhada junto com o projeto.

De forma simplificada:

**Configuração manual:**

```text
Desenvolvedor
      ↓
Cria a máquina virtual
      ↓
Configura o sistema
      ↓
Instala os programas
      ↓
Configura a rede
```

**Com Vagrant:**

```text
Vagrantfile
     ↓
vagrant up
     ↓
Ambiente criado e configurado
```

Isso facilita a criação de ambientes consistentes e reproduzíveis para desenvolvimento e testes.

---

## 2. Como o Vagrant funciona?

O Vagrant atua como uma camada de gerenciamento entre o usuário e a tecnologia responsável por executar o ambiente virtual.

Para entender seu funcionamento, é importante conhecer alguns componentes fundamentais.

### 2.1 Vagrantfile

O **Vagrantfile** é o arquivo principal de configuração de um projeto Vagrant.

Nele são descritas características do ambiente, como:

- box utilizada;
- configurações da máquina;
- rede;
- portas;
- pastas compartilhadas;
- provisionamento;
- configurações específicas do provider.

Um Vagrantfile básico possui uma estrutura semelhante a:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "nome-da-box"
end
```

A estrutura e as configurações desse arquivo serão apresentadas com mais detalhes no tópico **7 — Entendendo o Vagrantfile**.

### 2.2 Box

Uma **box** é um pacote que contém uma base previamente preparada para a criação do ambiente.

Por exemplo, uma box pode fornecer um sistema Linux que será utilizado como ponto de partida para a máquina criada pelo Vagrant.

Isso evita a necessidade de instalar manualmente um sistema operacional sempre que um novo ambiente for criado.

### 2.3 Provider

O **provider** é a tecnologia responsável por executar o ambiente.

O Vagrant gerencia esse provider e envia a ele as instruções necessárias para criar e controlar a máquina.

Entre os providers que podem ser utilizados estão:

- VirtualBox;
- Hyper-V;
- Docker.

Neste projeto, utilizaremos principalmente o **VirtualBox**.

### 2.4 Provisionamento

Depois de criar o ambiente, o Vagrant também pode executar automaticamente comandos para prepará-lo.

Esse processo é chamado de **provisionamento**.

Por exemplo, podemos configurar o Vagrant para criar uma máquina Linux e instalar automaticamente um servidor web.

O provisionamento será explicado detalhadamente no tópico **12 — Provisionamento**.

### 2.5 Fluxo básico de funcionamento

De forma simplificada, o funcionamento pode ser representado assim:

```text
Vagrantfile
     ↓
   Vagrant
     ↓
Box + Provider
     ↓
Ambiente virtual
     ↓
Provisionamento (opcional)
     ↓
Ambiente pronto para utilização
```

Na prática, o usuário define as configurações no **Vagrantfile** e utiliza os comandos do Vagrant para controlar o ambiente.

Um dos principais comandos é:

```bash
vagrant up
```

---

## 3. Pré-requisitos

Antes de começar a utilizar o Vagrant, é necessário preparar o computador com algumas ferramentas e configurações básicas.

Para acompanhar os exemplos deste guia, serão utilizados:

- **Vagrant** — ferramenta responsável por criar e gerenciar os ambientes;
- **VirtualBox** — utilizado como provider para executar as máquinas virtuais;
- **Windows 10 ou Windows 11**;
- **PowerShell, Prompt de Comando ou outro terminal** para executar os comandos;
- **Conexão com a internet**, principalmente durante a primeira criação da máquina virtual, pois será necessário baixar a box utilizada pelo Vagrant;
- **Virtualização de hardware habilitada** no computador.

### Vagrant e VirtualBox são a mesma coisa?

Não. Apesar de trabalharem juntos neste projeto, eles possuem funções diferentes.

```text
Vagrant
   ↓
Gerencia e automatiza o ambiente
   ↓
VirtualBox
   ↓
Executa a máquina virtual
```

O **VirtualBox** é responsável pela virtualização propriamente dita, enquanto o **Vagrant** fornece uma forma mais simples e automatizada de criar, configurar e controlar essas máquinas.

Por isso, para os exemplos deste guia, **os dois programas precisam estar instalados**.

o programa pode ainda não estar instalado ou seu executável pode não estar disponível no `PATH` do sistema.

A instalação das ferramentas será apresentada no próximo tópico.

## 4. Instalação

Para começar a usar o Vagrant, baixe o instalador ou pacote apropriado para sua plataforma no site [Hashicorp](http://developer.hashicorp.com/vagrant/install). Instale o pacote seguindo os procedimentos padrão para o seu sistema operacional.

O instalador adiciona automaticamente o diretório Vagrant ao seu PATH do sistema para que ele esteja disponível nos terminais. Se não for encontrado, faça logout e login novamente no sistema; esse é um problema comum no Windows.

## 5. Verificando a instalação

No PowerShell ou em outro terminal, execute:

```powershell
vagrant --version
```

Para verificar o VirtualBox:

```powershell
VBoxManage --version
```

Se os programas estiverem instalados e disponíveis no sistema, os comandos retornarão suas respectivas versões.

Caso apareça uma mensagem semelhante a:

```text
O termo 'vagrant' não é reconhecido como nome de cmdlet...
```

ou:

```text
O termo 'VBoxManage' não é reconhecido...
```

## 6. Criando o primeiro projeto

Crie uma pasta para o projeto e acesse esse diretório pelo terminal:

```bash
mkdir meu-primeiro-vagrant
cd meu-primeiro-vagrant
```

Inicialize um novo projeto Vagrant:

```bash
vagrant init
```

Esse comando cria um arquivo chamado Vagrantfile no diretório atual. O arquivo será utilizado para definir as configurações da máquina virtual.

No começo o projeto terá uma estrutura semelhante a:

```bash
meu-primeiro-vagrant/
└── Vagrantfile
```

Agora, o Vagrantfile pode ser configurado para utilizar uma box como sistema base da máquina virtual.

Depois de configurar o Vagrantfile, a máquina pode ser criada e iniciada utilizando o comando:

```bash
vagrant up
```

O Vagrant verificará se a box necessária está disponível localmente, e caso não esteja, ela será baixada e utilizada para criar a máquina através do provider configurado (Conceito de Idempotência).

## 7. Entendendo o Vagrantfile

O Vagrantfile deve descrever o tipo de máquina necessária para um projeto e como configurar e provisionar essas máquinas. O Vagrant foi projetado para ser executado com um Vagrantfile por projeto, e o Vagrantfile deve ser incluído no controle de versão, permitindo que outros desenvolvedores envolvidos no projeto baixem o código, executem o comando vagrant up e continuem trabalhando. Os Vagrantfiles são portáteis em todas as plataformas suportadas pelo Vagrant. A sintaxe dos Vagrantfiles é Ruby , mas o conhecimento da linguagem de programação Ruby não é necessário para fazer modificações no Vagrantfile, já que se trata principalmente de atribuição de variáveis ​​simples.

O arquivo utiliza a sintaxe da linguagem Ruby, mas não é necessário conhecimento avançado da linguagem para realizar as configurações básicas utilizadas pelo Vagrant.

Um Vagrantfile básico possui uma estrutura semelhante a:

```ruby
Vagrant.configure("2") do |config|
config.vm.box = "ubuntu/jammy64"
end
```


A instrução Vagrant.configure("2") inicia o bloco de configuração do Vagrant. O objeto config é utilizado para definir as configurações da máquina virtual.

config.vm.box define a box que será utilizada como base para criar a máquina virtual.

As configurações do Vagrantfile são lidas pelo Vagrant quando comandos como vagrant up são executados. A partir delas, o Vagrant utiliza o provider definido para criar e configurar a máquina virtual.

O Vagrantfile também pode conter diferentes configurações para o ambiente, permitindo definir características da máquina, rede, armazenamento e processos de provisionamento. Essas configurações serão apresentadas nos tópicos específicos deste guia.

### 7.1 Estrutura básica

A estrutura básica de um Vagrantfile pode ser entendida da seguinte forma:

```bash
Vagrantfile
↓
Vagrant.configure()
↓
Bloco de configuração
↓
Configurações da máquina
↓
Vagrant utiliza essas configurações
↓
Provider cria e configura a máquina virtual
```

## 8. Criando a máquina virtual

Com o Vagrantfile configurado, a máquina virtual pode ser criada e iniciada utilizando o comando:

```bash
vagrant up
```
Ao executar esse comando, o Vagrant lê as configurações definidas no Vagrantfile e utiliza o provider configurado para criar a máquina virtual.

Caso a box especificada ainda não esteja disponível localmente, o Vagrant realiza o download da box antes de utilizá-la.

Após a execução do comando, a máquina virtual estará em operação e poderá ser acessada utilizando o comando vagrant ssh, que será apresentado no próximo tópico.

## 9. Acessando a máquina virtual

Após criar a máquina virtual com o comando `vagrant up`, o Vagrant permite acessar o terminal da máquina através do protocolo SSH, sem a necessidade de abrir a interface gráfica do VirtualBox ou outro virtualizador.

### 9.1 Conectando via SSH

Para o acesso da máquina virtual criada, abrimos o terminal do computador na pasta onde o Vagrantfile está localizado e executamos:

```bash
vagrant ssh
```

O Vagrant utilizará as chaves SSH geradas durante a inicialização para autenticar o acesso. Se a conexão for bem-sucedida, o prompt do terminal mudará indicando que você está dentro do Linux:

```bash
vagrant@<hostname>:~$
```

### 9.2 Encerrando sessão SSH

Para sair do terminal da máquina virtual e retornar ao terminal do computador, realizamos o comando:

```bash
exit
```

## 10. Compartilhamento de arquivos

O Vagrant possui um recurso nativo chamado Synced Folders (Pastas Sincronizadas). Que permite que arquivos criados ou modificados no seu computador host sejam refletidos instantaneamente dentro da máquina virtual e vice-versa.

Por padrão, a pasta raiz do seu computador onde está o Vagrantfile é mapeada dentro da máquina virtual, normalmente no diretório: 

```bash
/vagrant
```

### 10.1 Acessando a pasta compartilhada

Acesse na máquina virtual utilizando:

```bash
vagrant ssh
```

Depois, acesse o diretório compartilhado:

```bash
cd /vagrant
```

Visualize os arquivos presentes na pasta:

```bash
ls
```

### 10.2 Teste prático: Criando arquivo na VM

Dentro da máquina virtual, acesse a pasta compartilhada:

```bash
cd /vagrant
```

Em seguida, crie um arquivo de teste:

```bash
echo "<Conteúdo>" > <nome_arquivo>.txt
```

Verifique se o arquivo foi criado:

```bash
ls
```

Verifique o conteúdo:

```bash
cat <nome_arquivo>.txt
```

### 10.3 Configurando uma pasta compartilhada

Além da pasta `/vagrant`, também é possível configurar outras pastas compartilhadas no `Vagrantfile`.

```bash
config.vm.synced_folder "./pasta", "/home/vagrant/pasta"
```

- `./pasta` representa a pasta no computador host;
- `/home/vagrant/pasta` representa o local onde ela será disponibilizada dentro da máquina virtual.

## 11. Configuração de rede

O Vagrant permite configurar diferentes tipos de rede para a máquina. Essa configuração determina como a máquina virtual poderá se comunicar com o computador host, outras máquinas virtuais e dispositivos presentes na rede.

Os principais tipos de configuração de rede utilizados pelo Vagrant são:

- NAT: permite que a máquina virtual acesse a internet utilizando a conexão do computador host.
- Private Network: cria uma rede privada entre o computador host e a máquina virtual.
- Public Network: conecta a máquina virtual à rede física utilizando o modo Bridge.

### 11.1 Rede privada

Com a rede privada é possível definir um endereço IP específico para a máquina virtual.

Podemos usar no `Vagrantfile`:

```bash
config.vm.network "private_network", ip: "192.168.56.10"
```

Esse tipo de configuração é útil quando é necessário estabelecer uma comunicação entre o computador host e a máquina virtual utilizando um endereço IP específico, sem disponibilizar diretamente a máquina para toda a rede física. E caso exista um servidor web funcionando na máquina virtual, ele poderá ser acessado através do endereço:

```bash
192.168.56.10
```

### 11.2 Rede pública

O Vagrant também pode conectar a máquina virtual diretamente à rede física utilizando o modo Bridge.

Para isso, podemos utilizar:

```bash
config.vm.network "public_network"
```

Nesse modo, a máquina virtual funciona como um dispositivo conectado à mesma rede física do computador host.

Durante o comando:

```bash
vagrant up
```

o Vagrant poderá solicitar que o usuário escolha qual interface de rede deverá ser utilizada.

Está configuração de rede pública é útil quando precisamos que a máquina virtual seja acessível por outros dispositivos conectados à mesma rede. 

Por exemplo, computadores conectados ao mesmo roteador podem conseguir se comunicar com a máquina virtual, dependendo das configurações de rede e firewall.

## 12. Provisionamento

Provisionamento é o processo de configurar automaticamente o ambiente dentro da máquina virtual assim que ela é criada, sem precisar entrar na VM e fazer tudo manualmente.

O provisionamento é feito a partir dos **Provisioners**. Eles garantem a instalação automática, alterações de configuração e outras atividades na VM como parte do processo `vagrant up`. O Vagrant fornece várias opções para provisionamento, de shell script até configurações avançadas:

- **Shell**: Executa scripts shell (bash) diretamente na VM. É o mais simples e direto.
- **Ansible**: Usa playbooks Ansible para configurar a máquina (executado a partir do host).
- **Puppet**: Usa manifests Puppet.
- **Docker**: Provisiona containers Docker dentro da VM.
- **File**: Copia arquivos do host para dentro da VM.

### Provisionando um ambiente Node.js usando shell scripting

#### 1. Estrutura de pasta

Crie a estrutura de pasta a seguir:


```markdown
projeto-node/
├── Vagrantfile
└── provision.sh
```

#### 2. Vagrantfile

Crie o arquivo **Vagrantfile** com o conteúdo a seguir:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "dev-node"

  # Encaminha a porta padrão usada por muitos servidores Node (ex: Express)
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  # Recursos da VM (1GB de RAM e 1 CPU)
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end

  # Provisionamento via script externo (provision.sh)
  config.vm.provision "shell", path: "provision.sh"
end
```

#### 3. Shell scripting

Crie o arquivo **provision.sh** e adicione o conteúdo a seguir:

```bash
#!/bin/bash

echo ">> Atualizando pacotes..."
apt-get update

echo ">> Instalando Node.js..."
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt-get install -y nodejs

echo ">> Verificando instalação..."
node -v
npm -v

echo ">> Provisionamento concluído!"
```

#### 4. Testando o provisionamento

Suba a máquina virtual:

```bash
vagrant up
```

Acesse a VM via SSH:

```bash
vagrant ssh
```

Confirme que o Node.js está instalado:

```bash
node -v
```

## 13. Principais Comandos

| Comando | Descrição |
|---|---|
| `vagrant --version` | Mostra a versão do Vagrant instalada. |
| `vagrant help` | Lista todos os comandos disponíveis. |
| `vagrant help <comando>` | Mostra informações detalhadas sobre um comando específico. |
| `vagrant init` | Cria um novo **Vagrantfile** no diretório atual. |
| `vagrant up` | Cria e inicia a máquina virtual (VM). Se for executada pela primeira vez, baixa a box e roda o provisionamento automaticamente. |
| `vagrant ssh` | Conecta ao terminal da VM via SSH. |
| `vagrant status` | Mostra o status atual da VM. |
| `vagrant global-status` | Mostra todas as VMs gerenciadas pelo Vagrant no computador. |
| `vagrant halt` | Desliga a VM mantendo o disco e as configurações salvas. |
| `vagrant reload` | Reinicia a VM aplicando as alterações feitas no **Vagrantfile** sem precisar destruir e recriar a máquina. |
| `vagrant suspend` | Pausa a VM salvando o estado atual de memória em disco. |
| `vagrant resume` | Retoma a VM que estava suspensa (`vagrant suspend`). |
| `vagrant destroy` | Remove completamente a VM, liberando todos os recursos usados por ela. |
| `vagrant provision` | Reexecuta os scripts e ferramentas de provisionamento informados no **Vagrantfile** em uma VM já criada e executando. |
| `vagrant box list` | Lista todas as boxes já baixadas no computador. |
| `vagrant box add <nome>` | Baixa e adiciona uma nova box ao sistema sem criar a VM com ela ainda. |
| `vagrant box remove <nome>` | Remove uma box baixada liberando espaço em disco. |
| `vagrant box update` | Verifica e baixa atualizações disponíveis para a box usada no projeto atual. |
| `vagrant validate` | Verifica se o **Vagrantfile** está sintaticamente correto sem precisar subir a VM. |

## 14. Encerrando e Removendo a Máquina

Utilizando como base a VM configurada com Node.js que criamos na [seção 12](#12-provisionamento), vamos encerrar e remover a máquina, usando os seguintes comandos: `exit`, `vagrant halt` e `vagrant destroy`.

### Saindo do Terminal da VM

Para sair do terminal da VM e voltar ao do computador host:

```bash
exit
```

### Encerrando a VM

Para encerrar a VM (nesta etapa, o disco, o Node.js e as configurações ainda continuam):

```bash
vagrant halt
```

### Removendo a VM

Para remover a VM:

```bash
vagrant destroy
```

O Vagrant vai pedir confirmação, então digite `y` e aperte Enter para confirmar. Depois do destroy, o disco virtual da VM é apagado, além da sua configuração de rede específica. O **Vagrantfile** e o script provision.sh continuam existindo na pasta do projeto.
