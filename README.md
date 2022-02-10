## Criação das máquinas virtuais

Para esse projeto são necessárias duas máquinas virtuais, optei por usar o [VirtualBox](https://www.virtualbox.org/) para criar o ambiente.

Na primeira máquina será instalado o [Open Source MANO](https://osm.etsi.org/).

Na segunda máquina será instalado o [OpenStack](https://www.openstack.org/).

#### Configuração das máquinas:

- **Mínimo:** 2 CPUs, 6GB RAM, 40GB de espaço em disco
- **Recomendado:** 2 CPUs, 8GB RAM, 40GB de espaço em disco
- **Sistema operacional:** [Ubuntu LTS](https://ubuntu.com/download/desktop)

**OBS:** Nas configurações de rede, defina a placa em modo bridge para que as máquinas possam comunicar entre si.

## Instalando o Open Source MANO

Antes de instalar o OSM precisamos ter o [curl](https://curl.se/) na nossa máquina, para isso execute o seguinte comando:

```
$ sudo apt install curl
```

Depois execute os comandos disponíveis na [documentação](https://osm.etsi.org/docs/user-guide/01-quickstart.html) do OSM para instala-lo:

```
$ wget https://osm-download.etsi.org/ftp/osm-11.0-eleven/install_osm.sh
$ chmod +x install_osm.sh
$ ./install_osm.sh 2>&1 | tee osm_install_log.txt
```

Para verificar se o OSM foi instalado com sucesso abra o navegador na URL `http://1.2.3.4/login`, substituindo "1.2.3.4" pelo IP da sua máquina.

Para obter seu IP:

```
$ ip addr show
```

**OBS:** O usuário e senha padrão é "admin".

Caso ocorra algum erro você pode checar o arquivo `osm_install_log.txt` para visualizar o log de instalação.

## Instalando o OpenStack

O OpenStack deve ser utilizado por um usuário não administrador, então vamos criar um usuário "stack" e garantir a ele privilégios de administrador.

```
$ sudo useradd -s /bin/bash -d /opt/stack -m stack
$ echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
$ sudo -u stack -i
```

O OpenStack será baixado de um repositório do OpenDev portanto precisamos baixar o git:

```
$ sudo apt install git
```

Baixando o OpenStack:

```
$ git clone https://opendev.org/openstack/devstack
$ cd devstack
```

Vamos usar o [nano](https://www.nano-editor.org/) para criar um arquivo de configuração `local.conf`.

```
$ sudo nano local.conf
```

Um editor será aberto no terminal, cole o seguinte código dentro dele:

```
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```

**OBS:** Use `ctrl + o` para salvar o arquivo, aperte enter para confirmar o nome e em seguide use `ctrl + x` para fechar o nano.

Por fim, execute o arquivo de instalação do OpenStack:

```
$ ./stack.sh
```

Para confirmar a instalação precisamos acessar o [Horizon](https://docs.openstack.org/horizon/latest/), para isso execute o comando `$ ip addr show`, cole o endereço ip da máquina no navegador e faça login usando "admin" e "secret" como usuário e senha.

## Conectando o OSM com o OpenStack

Abra o dashboard do OSM e procure por "VIM Accounts", em seguida clique em "New VIM".

Minhas configurações ficaram assim:

![Configurações da conta VIM.](/images/osm-vim-account-info.png)

**OBS:** O campo "URL" é o endereço onde o OpenStack está rodando localmente + `/identity/v3`, geralmente esse endereço é `192.168.15.107`.

**OBS:** O campo "Tenant name" é o nome do projeto no OpenStack, geralmente é "admin", podemos também criar novos projetos com outros nomes.

## Adicionando uma VNF

O OSM possui um [servidor FTP](https://osm-download.etsi.org/ftp/Packages/examples/) onde disponibilizam exemplos de NFVs, nesse caso baixei a "cirros_alarm_ns.tar.gz" e "cirros_alarm_vnf.tar.gz" para testar.
