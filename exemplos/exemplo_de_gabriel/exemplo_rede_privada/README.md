# Exemplo prático — Rede privada entre host e máquina virtual

Este exemplo cria uma rede privada, atribuindo um IP fixo à máquina virtual. Isso facilita o acesso direto a partir do computador, sem depender de redirecionamento de portas (`forwarded_port`).

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

Após a criação da máquina, ela pode ser acessada diretamente pelo endereço:

```text
192.168.56.10
```

Para testar a conectividade a partir do computador:

```bash
ping 192.168.56.10
```

## Principais comandos

Acessar a máquina virtual:

```bash
vagrant ssh
```

Verificar o estado da máquina:

```bash
vagrant status
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

```text
Vagrantfile
```
