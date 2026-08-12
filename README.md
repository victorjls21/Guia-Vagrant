# Guia-Vagrant
Guia prático de utilização do Vagrant desenvolvido para a disciplina de Infraestrutura como Código (IaC).
# Guia Prático de Vagrant

Este repositório apresenta um guia prático para instalação, configuração e utilização do Vagrant, desde a preparação do ambiente até a criação e o gerenciamento de máquinas virtuais.

## Sumário

1. [O que é Vagrant?](#1-o-que-é-vagrant)
2. [Como o Vagrant funciona?](#2-como-o-vagrant-funciona)
3. [Pré-requisitos](#3-pré-requisitos)
4. Instalação
5. Verificando a instalação
6. Criando o primeiro projeto
7. Entendendo o Vagrantfile
8. Criando a máquina virtual
9. Acessando a máquina virtual
10. Compartilhamento de arquivos
11. Configuração de rede
12. Provisionamento
13. Principais comandos
14. Encerrando e removendo a máquina
15. [Exemplos práticos](#15-Exemplos-práticos)
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

### Como verificar se já estão instalados?

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

o programa pode ainda não estar instalado ou seu executável pode não estar disponível no `PATH` do sistema.

A instalação das ferramentas será apresentada no próximo tópico.

## 15. Exemplos práticos

Esta seção reúne exemplos completos de Vagrantfiles para cenários comuns do dia a dia. Cada exemplo pode ser copiado para uma pasta vazia e executado com `vagrant up`.

### 15.1 Servidor web com Apache

Este exemplo cria uma máquina Ubuntu, instala o Apache automaticamente e redireciona a porta `80` da VM para a porta `8080` do computador.

```ruby
Vagrant.configure("2") do |config|

  # Define a box que será utilizada
  config.vm.box = "ubuntu/jammy64"

  # Define o nome da máquina
  config.vm.hostname = "servidor-web"

  # Encaminha a porta 80 da VM para a porta 8080 do computador
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Configura os recursos da máquina virtual
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end

  # Instala o Apache automaticamente
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y apache2
  SHELL

end
```

Após `vagrant up`, o resultado pode ser conferido em `http://localhost:8080`.

> Esse é o mesmo modelo utilizado no exemplo prático deste repositório, disponível em [`exemplos/exemplo_de_victor`](../exemplos/exemplo_de_victor).

### 15.2 Servidor com pasta compartilhada personalizada

Este exemplo mostra como sincronizar uma pasta específica do computador com uma pasta da máquina virtual, além da sincronização padrão.

```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "servidor-arquivos"

  # Compartilha a pasta "site" do host com "/var/www/html" na VM
  config.vm.synced_folder "./site", "/var/www/html"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end

end
```

Isso é útil quando se deseja editar arquivos no computador (por exemplo, em um editor de código) e ver as mudanças refletidas imediatamente dentro da máquina virtual.

### 15.3 Ambiente com múltiplas máquinas virtuais

O Vagrant também permite descrever mais de uma máquina no mesmo Vagrantfile, o que é útil para simular, por exemplo, um servidor de aplicação e um servidor de banco de dados.

```ruby
Vagrant.configure("2") do |config|

  # Máquina 1: servidor web
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/jammy64"
    web.vm.hostname = "web"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
  end

  # Máquina 2: banco de dados
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/jammy64"
    db.vm.hostname = "db"
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
  end

end
```

Com essa configuração, comandos como `vagrant up`, `vagrant ssh` e `vagrant halt` podem ser direcionados a uma máquina específica, informando seu nome:

```bash
vagrant ssh web
vagrant halt db
```

### 15.4 Rede privada entre host e máquina virtual

Este exemplo cria uma rede privada, atribuindo um IP fixo à máquina virtual, o que facilita o acesso direto sem depender de redirecionamento de portas.

```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "servidor-privado"

  # Cria uma rede privada com IP fixo
  config.vm.network "private_network", ip: "192.168.56.10"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

end
```

Após `vagrant up`, a máquina pode ser acessada diretamente pelo endereço `192.168.56.10`.

---