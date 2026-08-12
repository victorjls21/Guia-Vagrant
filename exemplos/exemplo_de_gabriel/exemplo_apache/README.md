# Exemplo prático — Servidor web com Apache

Este exemplo cria uma máquina virtual Ubuntu e instala o Apache automaticamente durante o provisionamento.

A máquina é configurada com:

- 1 GB de memória RAM;
- 1 CPU;
- Apache instalado automaticamente;
- redirecionamento da porta `80` da máquina virtual para a porta `8080` do computador.

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

Após a criação da máquina, acesse no navegador:

```text
http://localhost:8080
```

Se tudo estiver funcionando corretamente, será exibida a página padrão do Apache.

## Principais comandos

Verificar o estado da máquina:

```bash
vagrant status
```

Acessar a máquina virtual:

```bash
vagrant ssh
```

Desligar a máquina:

```bash
vagrant halt
```

Remover a máquina:

```bash
vagrant destroy
```

## Arquivo utilizado

A configuração da máquina está definida no arquivo:

```text
Vagrantfile
```
