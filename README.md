# Montando a .ISO Do pfSense com base no códifo-fonte

Este guia foi desenvolvido para funcionar com a versão RELENG_2_7_3 ou 2_7_4 não oficial da pfSense.

Em alguns pontos do repositório pfsense, existe muitas menções à palavra "pfSense" nos códigos e isso gera uma "trava", é preciso definir um nome de produto antes de iniciarmos, vamos utilizar "libreSense".

Crie uma conta no GITHUB, vamos criar um FORK dos seguintes repositórios oficiais da pfSense:

[https://github.com/pfsense/pfsense](https://github.com/pfsense/pfsense.git)

[https://github.com/pfsense/freebsd-ports](https://github.com/pfsense/FreeBSD-ports.git)

[https://github.com/pfsense/freebsd-src](https://github.com/pfsense/FreeBSD-src.git)

Para criar um FORK cliquem em:

![image](https://github.com/user-attachments/assets/3b35472f-6c8c-47e5-a92f-85b4023c999c)

Certifique-se de que os FORK'S são completos, ou seja, não apenas a BRANCH MASTER/DEVEL(DESMARQUE "Copy the master branch only), segue exemplo:

![image](https://github.com/user-attachments/assets/9f883ad4-4629-4aaf-b4ba-5eeb2a658ecb)

Vá nas configurações do GITHUB Web, na àrea de Desenvolvimento, a última opção:

![image](https://github.com/user-attachments/assets/55f46ca8-08c5-4cae-9d75-c06d15b834b1)

Vá em "tokens classic" e gere um:

![image](https://github.com/user-attachments/assets/ee3cb9cb-247d-43e0-9bd6-750db349b8f5)

SALVE ESTA SENHA!!!

Vamos editar algumas informações nos arquivos, para isso, instale o GIT https://git-scm.com/downloads, crie uma pasta na área de trabalho com o nome do seu produto, no meu caso libreSense:

![image](https://github.com/user-attachments/assets/28831727-7acf-478b-b90b-3de69ce73cb3)

Dentro desta pasta, com o botão direito, clique em "Open GIT Bash Here":

![image](https://github.com/user-attachments/assets/289cb245-55bf-405b-9c16-6ddedb1ce8c9)

![image](https://github.com/user-attachments/assets/519419fa-0f9e-4c29-894e-95da60ad0e43)

Vamos baixar os arquivos do repositório do nosso GITHUB, para pegar as informações, em nosso repositório, pegue o link de download em "code":

![image](https://github.com/user-attachments/assets/a96b3498-f824-4ff4-b4ef-cd960c86ed41)

Siga os comandos(aguarde a finalização de cada um):

- git clone https://github.com/Christopher-YeTI/pfsense.git
- git clone https://github.com/Christopher-YeTI/FreeBSD-ports.git
- git clone https://github.com/Christopher-YeTI/FreeBSD-src.git

Agora vamos realizar o login da nossa conta do GITHUB em nosso BASH com os seguintes comandos:

- git config --global user.name "Christopher-YeTI"
- git config --global user.email christopher@yeti.tec.br

se for solicitando uma senha, informe a key gerada no GITHUB Web.

---TUDO PRONTO PARA EDITAR OS ARQUIVOS---

### FreeBSD Source
Dentro da pasta Desktop\libreSense\FreeBSD-src\ execute o GIT Bash, em seguida os comandos:

- git branch RELENG_2_7_3
- git checkout RELENG_2_7_3

Isso vai criar uma nova BRANCH em nosso repositório e com o checkout vamos mudar para ela, altere os arquivos a seguir:
Na pasta "/release/conf/", renomear os arquivos que comecem com "pfSense" para "libreSense"(exemplo, "pfSense_install_src.conf" => "libreSense_install_src.conf")

![image](https://github.com/user-attachments/assets/2586f558-5b31-4d6c-a5f7-13cd3233947d)

Renomeie o arquivo "/sys/amd64/conf/pfSense" to "/sys/amd64/conf/libreSense"

![image](https://github.com/user-attachments/assets/16badb26-207d-41b4-9123-3df31ae51e14)

Edite o arquivo "/tools/tools/crypto/Makefile" removendo as palavras "cryptotest cryptostats" do comando "PROGS"

![image](https://github.com/user-attachments/assets/095c1e2b-166a-4894-8612-3a4299414950)

Execute os seguintes comandos no GIT BASH:

- git add . && git commit -m “Alterações para nova versão de compilação”
- git push origin RELENG_2_7_3

### FreeBSD Ports
Dentro da pasta Desktop\libreSense\FreeBSD-ports\ execute o GIT Bash, em seguida os comandos:

- git branch RELENG_2_7_3
- git checkout RELENG_2_7_3

Edite o arquivo "/sysutils/pfSense-upgrade/Makefile" removendo a linha "RUN_DEPENDS+= pfSense-repoc>=0:sysutils/pfSense-repoc"

![image](https://github.com/user-attachments/assets/ab78e9ee-8139-460a-99a5-79d876a3e6f6)

Edite o arquivo "/security/pfSense/pkg-plist" removendo todas as linhas que comecem com "%%DATADIR%%/keys"

![image](https://github.com/user-attachments/assets/64d0ac1f-5cd2-4f98-9af1-f505b21073bf)

Edite o arquivo "/sysutils/pfSense-repo/Makefile" alterando o "srv" para "none"

![image](https://github.com/user-attachments/assets/58e1b744-6c6a-4777-a8aa-5a3fbb83af81)

Edite o arquivo "/security/pfSense/Makefile"
  - Renomeie a variavel "USE_GITLAB" >> "USE_GITHUB"
  - Renomeie a variavel "GL_ACCOUNT" >> "GH_ACCOUNT" Aqui devemos informar nosso usuário do GITHUB (Christopher-YeTI)
  - Renomeie a variavel "GL_COMMIT" >> "GH_TAGNAME"
  - Remova a linha "GL_SITE= https://gitlab.netgate.com"
  - Remova a linha "pfSense-gnid>=0:security/pfSense-gnid \" da varavel "RUN_DEPENDS", se, ela existir

Execute os seguintes comandos no GIT BASH:

- git add . && git commit -m “Alterações para nova versão de compilação”
- git push origin RELENG_2_7_3

### pfSense GUI
Dentro da pasta Desktop\libreSense\pfSense\ execute o GIT Bash, em seguida os comandos:

- git branch RELENG_2_7_3
- git checkout RELENG_2_7_3

Na pasta "/tools/templates/pkg_repos/" renomeie o arquivo "pfSense-repo.conf" to "libreSense-repo.conf"

![image](https://github.com/user-attachments/assets/1e324e6d-f95c-4839-9f73-3ca8ed5c0d5b)

Edite o arquivo "/src/etc/inc/globals.inc" altere o "product_name" para "libreSense", e "pkg_prefix" para "libreSense-pkg-"

![image](https://github.com/user-attachments/assets/9e5e841c-6f01-4fcd-abea-fd23f55ba0a2)

![image](https://github.com/user-attachments/assets/56695890-a2e8-4800-a932-2000d9461816)

Na pasta "/src/usr/local/share/" renomeie a pasta "pfSense" para "libreSense"

![image](https://github.com/user-attachments/assets/766ae045-0cce-4ad2-9c69-d38b2ff495d9)

Na pasta "/src/etc/" renomeie o arquivo "pfSense-ddb.conf" e "pfSense-devd.conf" para "libreSense-ddb.conf" e "libreSense-devd.conf"

![image](https://github.com/user-attachments/assets/7d1d24b8-e5fb-460f-a140-756699c3ce7e)

Edite o arquivo "/tools/templates/core_pkg/base/pkg-plist" removendo a linha "share/%%PRODUCT_NAME%%/initial.txz"

![image](https://github.com/user-attachments/assets/eccd801c-27e4-4d1b-b782-60ad533e81c3)

Edite o arquivo "/tools/builder_common.sh" and "/tools/builder_defaults.sh" aplicando a seguinte "correção":
[following pull request](https://github.com/pfsense/pfsense/pull/4721) ([see how](https://www.andrewkroh.com/development/2018/01/17/testing-github-pull-requests-using-git-patches.html))

Caso este documento acima não esteja mais disponível, utilize as versão do meu RELENG_2_7_2, estão com as alterações aplicadas, basta substituí-las

![image](https://github.com/user-attachments/assets/89c0f3ec-0b86-41b1-8f04-88b5b82450d0)

Edite o arquivo "/tools/builder_defaults.sh" Removendo "drm2" e "ndis" da varavel "MODULES_OVERRIDE_amd64"
Neste mesmo arquivo, altere a informação da variavel "PKG_REPO_BRANCH_RELEASE" para v_2_7_3

![image](https://github.com/user-attachments/assets/644e674b-0696-410b-8107-f6d94a3d9fd2)

### Setup gamer
A versão de instalação que vamos tentar realizar a build é baseada no FreeBSD 15.0-RELEASE baixe a .iso do mesmo em https://download.freebsd.org/snapshots/amd64/amd64/ISO-IMAGES/15.0/ versão ADM64 disc1.iso, pode ser a mais recente

Recomendações de Hardeware:
- 26GB de memóriam RAM;
- 50GB de disco;
- 16 vCpu.

Instale a versão ZFS 

![image](https://github.com/user-attachments/assets/9326f3ef-44c3-4483-ad58-5ada345d2b9a)

### Configurando nosso FreeBSD
Siga os comandos:

- echo PermitRootLogin yes >> /etc/ssh/sshd_config
- service sshd restart

Utilizando uma conexão SSH execute os seguintes comandos:

- freebsd-update fetch
- freebsd-update install
- pkg install -y pkg vim nano emacs git nginx poudriere-devel rsync sudo vmdktool curl qemu-user-static gtar xmlstarlet pkgconf openssl portsnap htop screen wget mmv open-vm-tools py311-gdbm py311-sqlite3 py311-tkinter python3 unbound cmake llvm libffi pkgconf 
- mkdir -p /var/db/portsnap
- portsnap fetch extract
- dd if=/dev/zero of=/root/swap.bin bs=1M count=16384
- chmod 0600 /root/swap.bin 
- mdconfig -a -t vnode -f /root/swap.bin -u 0   
- echo 'swapfile="/root/swap.bin"' >> /etc/rc.conf 
- swapon /dev/md0  
- nano /etc/make.conf

Informações:
            DEFAULT_VERSIONS+=ssl=libressl
            DEFAULT_VERSIONS+=ssl=openssl
            DEFAULT_VERSIONS+=perl5=5.36

Para salvar com o editor "nano" pressione CTRL+o, enter para salvar e para sair CTRL+x
Editor "vi" pressiona "a" para iniciar as escritas, ESC para sair da escrita, pressione ":" "wq" e enter para sair e salvar

Vamos utilizar "janela" chamadas de "Screen", se o acesso via SSH for encerrado de forma inesperada, os comandos em execução não serão perdidos:

- screen -S build
- screen -ls
- sceen -r xxxx 

Com a janela iniciada daremos continuidade aos comandos:
- cd /usr/local/www/nginx/
- rm -rf *
- mkdir -p packages 
- mkdir -p poudriere-build 
- ln -s /root/pfsense/tmp/${libreSense}_${v2_7_3}_amd64-core/.latest packages/${libreSense}_${v2_7_3}_amd64-core
- ln -s /usr/local/poudriere/data/packages/${libreSense}_${v2_7_3}_amd64-${libreSense}_${v2_7_3} packages/${libreSense}_${v2_7_3}_amd64-${libreSense}_${v2_7_3} 
- ln -s /usr/local/poudriere/data/logs/bulk/${libreSense}_${v2_7_3}_amd64-${libreSense}_${v2_7_3}/latest poudriere
- sed -i '' 's+/usr/local/www/nginx;+/usr/local/www/nginx; autoindex on;+g' /usr/local/etc/nginx/nginx.conf
- pw group mod wheel -m www
- echo nginx_enable=\"YES\" >> /etc/rc.conf
- service nginx restart
- mkdir -p /root/sign/
- cd /root/sign/
- openssl genrsa -out repo.key 2048
- chmod 0400 repo.key
- openssl rsa -in repo.key -out repo.pub -pubout
- printf "function: sha256\nfingerprint: \"$(sha256 -q repo.pub)\"\n" > fingerprint
- curl -o /root/sign/sign.sh https://raw.githubusercontent.com/freebsd/pkg/master/scripts/sign.sh
- sed -i "" 's+ repo\.+ /root/sign/repo\.+g' /root/sign/sign.sh
- chmod +x /root/sign/sign.sh
- cd /root
- git clone --branch RELENG_2_7_3 https://github.com/Christopher-YeTI/pfsense.git
- cd pfsense/
- rm /root/pfsense/src/usr/local/share/libreSense/keys/pkg/trusted/*
- cp /root/sign/fingerprint src/usr/local/share/libreSense/keys/pkg/trusted/fingerprint
- nano build.conf
   - export PRODUCT_NAME="libreSense"
   - export FREEBSD_REPO_BASE=https://github.com/Christopher-YeTI/FreeBSD-src.git
   - export POUDRIERE_PORTS_GIT_URL=https://github.com/Christopher-YeTI/FreeBSD-ports.git
   - export FREEBSD_BRANCH=RELENG_2_7_3
   - export POUDRIERE_PORTS_GIT_BRANCH=RELENG_2_7_3
   - unset USE_PKG_REPO_STAGING
   - export DEFAULT_ARCH_LIST="amd64.amd64" 
   - export PKG_REPO_SIGNING_COMMAND="/root/sign/sign.sh ${PKG_REPO_SIGN_KEY}"
   - export myIPAddress=$(ifconfig -a | grep inet | grep '.'| head -1 | cut -d ' ' -f 2)
   - export PKG_REPO_SERVER_DEVEL="http://${myIPAddress}/packages"
   - export PKG_REPO_SERVER_RELEASE="http://${myIPAddress}/packages"
   - export PKG_REPO_SERVER_STAGING="http://${myIPAddress}/packages"
   - export MIRROR_TYPE="none"
   - export POUDRIERE_PFSENSE_SRC_REPO=$(git config --get remote.origin.url|sed 's/.*\/\(.*\)\.git/\1/g'|tr '[:upper:]' '[:lower:]')
   - export SRCCONF="/root/pfsense/tmp/FreeBSD-src/release/conf/${PRODUCT_NAME}_build_src.conf"

![image](https://github.com/user-attachments/assets/55e66c8c-47ae-4f8b-8b67-84b193f3c33e)

- chmod +x build.sh
- crontab -e
     * * * * * cp -R /usr/local/poudriere/data/logs/bulk/libreSense_v2_7_3_amd64-libreSense_v2_7_3/* /usr/local/www/nginx/poudriere-build/

![image](https://github.com/user-attachments/assets/9e1a27b1-0087-427e-990b-685719689d02)

contrab utiliza "vi" para editar o arquivo, pressiona "a" para escrever, ESC para sair da escrita : wq enter para salvar e sair
- service cron start
- cron_enable="YES"

### Iniciar a preparação dos arquivos buildWorld
- ./build.sh --setup-poudriere - Este procedimento pode levar de 3 a 4 horas, é possível acompanhar o log em "/root/pfsense/logs/poudriere.log"

### Iniciar a Build da ports
- ./build.sh --update-pkg-repo -  Este comando vai compilar todos os ports "selecionados" para o pfSense, este procedimento pode levar cerca de 5 a 6 horas
Para acompanharmos todo o procedimento e baixar log's que podem nos ajudar a resolver possíveis problemas na compilação dos ports, na WEB informe o IP do seu Servidor FreeBSD http://192.168.15.163/

![image](https://github.com/user-attachments/assets/9f542143-8e9e-4d82-9d8b-2e5ecac3eca3)

![image](https://github.com/user-attachments/assets/34dc52d6-072d-4d6b-aa93-55c08db9b7b4)

![image](https://github.com/user-attachments/assets/85b6206d-f563-49f8-87ae-6c7b3169c50f)

![image](https://github.com/user-attachments/assets/78efb47a-3255-4c2b-a925-57867e77a7a5)

### Algumas causas raízes possíveis de problemas na compilação dos PORTS: 
- Você está tentando construir uma versão desatualizada do pfSense e alguns arquivos "dist" não estão mais disponíveis no [distcache oficial do FreeBSD](http://distcache.FreeBSD.org/), resultando em erros na etapa "fetch"
- Neste caso, você pode atualizar o [`distinfo`](https://github.com/pfsense/FreeBSD-ports/blob/devel/print/texinfo/distinfo) de cada PORTS em questão em seu fork do GitHub de "FreeBSD ports", então você pode executar "./build.sh --update-poudriere-ports" para atualizar os arquivos
- Alternativa, você pode encontrar qualquer espelho dist antigo como [este](http://distfiles.icmpv6.org/distfiles/)), baixar os arquivos no servidor de FreeBSD em sua pasta correspondente (usando `wget`/`curl`), e então continuar a compilação. Lembre-se, caso o mesmo dê certo, faça o upload dos arquivos no GitHub

### Você precisa construir **TODOS** PORTS antes de prosseguir para a próxima etapa. Se não quiser criar uma PORT, você pode excluí-la removendo-a em [poudriere_bulk).](https://github.com/Christopher-YeTI/pfsense/blob/master/tools/conf/pfPorts/poudriere_bulk)

Comando para procurar determinado arquivo contento a informação no nome:

- find / -name "radius.c.rej" 2>/dev/null 

Comando para procurar uma palavra dentro de um arquivo no seguinte caminho:

- grep -rni "pear-Auth_RADIUS@php84" /usr/local/poudriere/ports/libreSense_v2_7_3/net/

----------------------------------------------------------------não consigo progredir------------------------------------------------------------------------

### Build kernel and create ISO

 Finally, you can build your customized firewall: `./build.sh --skip-final-rsync iso`. This command will build the kernel, then install ports on top of it and create the ISO file. Expect the command to run for ~one hour.
 
The build can the monitored from the two files in the `logs/` directory of pfSense GUI: 
- `buildworld.amd64`, `installworld.amd64` and `kernel.libreSense.amd64.log` will contain logs relative to the build of FreeBSD kernel.
- `install_pkg_install_ports.txt` contain logs relative to the installation of the ports. They are retrieved from the URL specified in the `build.conf` file.
- `isoimage.amd64` and `cloning.amd64.log` contain logs relative to the build of the ISO itself

At the end of the build, a compressed iso file (`.iso.gz`) will be present in `~pfsense/tmp/${product_name}/installer/`. You can extract it using `gunzip *.gz` if you need the plain `.iso`.

### If your encounter errors during ports or kernel build: possible root causes, and how to fix them:

One very possible root cause to why your build is failing, is related to how `Makefile` system works correlated with an unlucky timing. What happens is usually close to the following: 

1. Netgate build port XXX from `FreeBSD-ports`. On internal Netgate servers, poudriere marks the port as built and won't attempt to re-build it unless an update is made on the code ("config bump")
3. Few months later, ports owners update their build requirement (`distfile` update) / Netgate update its build environment (e.g., `openssl => opensll111` )
2. Netgate does NOT keep `FreeBSD-ports` in sync with the official FreeBSD ports repository. At this point, any attempt of rebuilding ports will fail...But it is fine for Netgate : the built packages are already there, and no rebuild will be made as long as no config bump is done on the ports. 
4. When you try to build ports yourself, some ports fail during build and you don't understand why.

The same applies for the content of `FreeBSD-src`. The recommended way to fix this issue is to simply re-synchronize modules that are failing with upstream, so that you will have an up-to-date ISO.

Also, Netgate has been accused of [performing *delayed open-sourcing*](https://github.com/doktornotor/pfsense-still-closedsource/blob/master/img/screenshot_bug8155_rebuilding_pfsense_kernel.png) in the past on `Freebsd-src`. It is unclear if it was due to users misunderstanding on how the build system works, due to Netgate shady practices, or due to a temporary maintenance. 
I haven't noticed any *delayed open sourcing* myself, but if that ever happens, you can merge FreeBSD sources with your branch directly.


# Differences between a genuine pfSense and your ISO

Your ISO is built the same way as pfSense ISO distributed by Netgate, and does contain the same code, with two major differences: your ISO does not include GNID nor pfSense-repoc.

## GNID

GNID is a binary (located at `/usr/sbin/gnid`) which is managing Netgate license for pfSense. This binary basically generates a unique Netgate ID for each genuine pfSense. 

The generated unique ID then become part of the default "User-Agent" when making HTTP requests with PHP (HTTP requests are used for fetching bogons, installing packages, displaying the first copyright message...etc). 

It is also required for accessing Netgate services on pfSense (such as ACB, or professional support)

How does this binary works is known *(a quick look to this binary with `radare2` show that it basically tries to fetch the platform it is running on, and if the platform is not Netgate hardware then it computes an ID using sha256 and MAC addresses of the device)*, but this program is property of Netgate and is closed-source (it is stored in the internal GitLab of Netgate). 

## pfSense-repoc

pfSense-repoc is a package containing multiple binaries. These binaries are in charge of configuring the right PKG repositories, depending on your licence and product (pfSense-CE, plus, etc..).

Like GNID, this package is property of Netgate and is closed-source.


Because your ISO does not contain pfSense-repoc nor GNID, you may not be able to retrieve bogons feed from Netgate, to use ACB, to install packages from official repositories or to ask for professional support to Netgate.
You also won't receive latest pfSense updates (which makes sense, since your ISO can't be called *pfSense software*..).

