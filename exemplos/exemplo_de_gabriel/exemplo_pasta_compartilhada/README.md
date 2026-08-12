# Exemplo prático — Pasta compartilhada personalizada

Este exemplo mostra como sincronizar uma pasta específica do computador com uma pasta da máquina virtual, além da sincronização padrão que o Vagrant já realiza.

A pasta `site`, presente neste diretório, é sincronizada com `/var/www/html` dentro da máquina virtual.

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

Depois, acesse a máquina virtual:

```bash
vagrant ssh
```

E confira o conteúdo sincronizado:

```bash
ls /var/www/html
cat /var/www/html/index.html
```

Qualquer alteração feita no arquivo `site/index.html`, no computador, é refletida automaticamente dentro da máquina virtual — não é necessário reiniciar a VM.

## Principais comandos

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

## Arquivos utilizados

```text
Vagrantfile
site/index.html
```
