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
> sudo apt install curl
```

Depois execute os comandos disponíveis na [documentação](https://osm.etsi.org/docs/user-guide/01-quickstart.html) do OSM para instala-lo:

```
> wget https://osm-download.etsi.org/ftp/osm-11.0-eleven/install_osm.sh
> chmod +x install_osm.sh
> ./install_osm.sh 2>&1 | tee osm_install_log.txt
```

Para verificar se o OSM foi instalado com sucesso abra o navegador na URL `http://1.2.3.4/login`, substituindo "1.2.3.4" pelo IP da sua máquina.

Para obter seu IP:

```
> ip addr show
```

**OBS:** O usuário e senha padrão é "admin".

Caso ocorra algum erro você pode checar o arquivo `osm_install_log.txt` para visualizar o log de instalação.
