# Exemplo de Vagrant — Rafael

Este exemplo cria uma máquina virtual Ubuntu utilizando o Vagrant e o VirtualBox, e delega o provisionamento ao **Ansible**, instalando e configurando o **Docker Engine** dentro da VM.

O objetivo é mostrar que o Vagrant não compete com ferramentas de gerência de configuração — ele cria a infraestrutura base (a VM) e passa a responsabilidade de instalar o software para o Ansible.

A máquina é configurada com:

- 2 GB de memória RAM;
- 2 CPUs;
- provisionamento via Ansible (`playbook.yml`), que instala:
  - Docker Engine, Docker CLI, containerd e docker-compose plugin;
  - habilita e inicia o serviço `docker`;
  - adiciona o usuário `vagrant` ao grupo `docker`;
  - roda um `docker run hello-world` para validar a instalação.

## Pré-requisitos

Além do Vagrant e do VirtualBox, é necessário ter o **Ansible** instalado na máquina host, já que o provisionamento usa `ansible` (não `ansible_local`):

```bash
sudo apt install ansible
```

> Alternativa: se preferir não instalar o Ansible localmente, basta trocar `provision "ansible"` por `provision "ansible_local"` no `Vagrantfile` — nesse caso o próprio Vagrant instala o Ansible dentro da VM antes de rodar o playbook.

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

O Vagrant vai baixar a box `ubuntu/jammy64`, criar a VM e em seguida rodar o `playbook.yml`, que instala o Docker automaticamente.

Para confirmar que o Docker está funcionando, acesse a VM e rode um container:

```bash
vagrant ssh
docker run hello-world
```

## Principais comandos

Verificar o estado da máquina:

```bash
vagrant status
```

Acessar a máquina virtual:

```bash
vagrant ssh
```

Reexecutar apenas o provisionamento (sem recriar a VM):

```bash
vagrant provision
```

Desligar a máquina:

```bash
vagrant halt
```

Destruir a máquina:

```bash
vagrant destroy
```

## Arquivos utilizados

- `Vagrantfile` — define o sistema operacional, os recursos da VM e aciona o provisionador Ansible.
- `playbook.yml` — playbook Ansible responsável por instalar e configurar o Docker Engine.
