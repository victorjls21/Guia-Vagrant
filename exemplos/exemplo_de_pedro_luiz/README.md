# Exemplo de Vagrant — Pedro Luiz

Este exemplo demonstra o acesso a uma máquina virtual Ubuntu utilizando o Vagrant e o VirtualBox, além do compartilhamento de arquivos e de duas configurações de rede: rede privada com IP estático e rede pública em modo Bridge.

## Configurações utilizadas

- Ubuntu Xenial 16.04;
- 1 GB de memória RAM;
- 1 CPU;
- hostname `servidor-vagrant`;
- compartilhamento da pasta do projeto através de `/vagrant`;
- rede privada com IP `192.168.56.10`;
- rede pública utilizando Bridge.

## Como executar

Na pasta deste exemplo, execute:

```bash
vagrant up
```

## Compartilhamento de arquivos

Após acessar a máquina:

```bash
vagrant ssh
```

Entre na pasta compartilhada:
```bash
cd /vagrant
```

Visualize os arquivos com:

```bash
ls -la
```

### Teste de compartilhamento

Dentro da máquina, crie um arquivo:

```bash
echo "Teste de compartilhamento" > arquivo_teste.txt
```

Verifique seu conteúdo:

```bash
cat arquivo_teste.txt
```

Também é possível fazer o inverso. Crie um arquivo na pasta pelo computador, como:

```bash
teste_windows.txt
```

Dentro da pasta compartilhada:

```bash
cd /vagrant
```

Verifique o conteúdo do arquivo criado:

```bash
cat teste_windows.txt
```

## Configuração de rede

### Rede Privada

A rede utiliza um endereço IP estático:

```bash
192.168.56.10
```

Para verificar as interfaces de rede, execute no terminal da máquina:

```bash
ip a
```

Ou verifique o IP configurado diretamente:

```bash
ip addr | grep 192.168.56.10
```

### Rede Pública (Bridge)

A rede pública foi configurada para obter um endereço IP dinâmico (DHCP) através do roteador da sua rede local.

### Seleção da Placa de Rede no Terminal

Durante o comando `vagrant up` ou `vagrant reload`, o Vagrant pode solicitar a escolha da interface de rede do seu computador:

```text
==> default: Available bridged network interfaces:
1) Realtek PCIe GbE Family Controller
2) VPN Ethernet Adapter
==> default: When choosing an interface, it is usually the one that is
==> default: being used to connect to the internet.
==> default: 
    default: Which interface should the network bridge to?
```

- Para responder, primeiro identifique qual placa de rede do seu computador está conectada à internet (geralmente a placa Wi-Fi ou a placa Ethernet/cabo). 

- Após, digite o número correspondente a essa placa e pressione Enter. 

- Assim a máquina virtual receberá um IP dinâmico atribuído pelo seu roteador.

Para verificar os endereços IP atribuídos, execute no terminal da máquina:

```bash
hostname -I
```

## Teste de Comunicação

Após identificar o endereço IP da máquina, é possível testar a comunicação a partir do computador.

Na rede privada:

```bash
ping 192.168.56.10
```

Na rede pública (Bridge), utilize o endereço atribuído à interface de IP Bridge:

```bash
ping <Endereço IP Bridge>
```

## Alteração no Vagrantfile

Sempre que alterar algo no Vagrantfile, primeiro saia do terminal da máquina com:

```bash
exit
```

Para reiniciar sua máquina e aplicar as novas mudanças, execute no terminal do computador o comando abaixo:

```bash
vagrant reload
```

## Encerrar Máquina

Após concluir os testes, você pode gerenciar o estado da máquina no terminal do computador.

- Desligar máquina virtual

```bash
vagrant halt
```

- Destruir máquina virtual

```bash
vagrant destroy
```

