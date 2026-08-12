# Exemplo prático — Múltiplas máquinas virtuais

Este exemplo mostra como descrever mais de uma máquina virtual em um único Vagrantfile, útil para simular, por exemplo, um servidor de aplicação (`web`) e um servidor de banco de dados (`db`).

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

Esse comando cria as duas máquinas definidas no Vagrantfile.

Para subir apenas uma delas:

```bash
vagrant up web
```

## Principais comandos

Como há mais de uma máquina, é necessário informar o nome dela na maioria dos comandos:

Acessar a máquina `web`:

```bash
vagrant ssh web
```

Acessar a máquina `db`:

```bash
vagrant ssh db
```

Verificar o estado de todas as máquinas:

```bash
vagrant status
```

Desligar apenas a máquina `db`:

```bash
vagrant halt db
```

Remover todas as máquinas:

```bash
vagrant destroy
```

## Arquivo utilizado

```text
Vagrantfile
```
