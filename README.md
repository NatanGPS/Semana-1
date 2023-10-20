Semana-1
# Documentação das atividades pedidas na semana um
## Primeiro desafio:
### Primeiro passo: foi necessário fazer a instalaçãop do Virtual Box no meu computador:
1° - Acessar o site https://www.virtualbox.org/;
<br>2° - Clicar na aba downloads;
<br>3° - Selecionar qual Sistema Operacional voce deseja instalar e fazer o download do mesmo (windows 10 no meu caso);
<br>4° - Executar o programa e seguir os passos que são dados por ele para concluir a instalação;

### Segundo passo: com o Virtual Box já instalado precisamos baixar e instalar a iso do linux que vamos emular em nossa maquina virtual.
    
<br>1° - Acessar o site da oracle para baixar a iso que vamos utilizar: https://yum.oracle.com/oracle-linux-isos.html
<br>2° - Baixar a versão mais atual do linux R8 geralmente nomeada da seguinte forma "OracleLinuc-R8-U(numero da versão)-x86_64-dvd.iso"
<br>3° - Com a iso já baixada, abra o virtual box e clique na opção "New"
<br>4° - Utilizando o tutorial do seguinte site a seguir para terminar de realizar a instalação  e a cofiguração do linux em sua maquina virtual: https://linuxhint.com/install_oracle_linux_8_virtual_box/.


## Segundo desafio:
### Primeiro passo: Precisamos instalar novamente nossa iso, porém agora queremos instalar sem GUI
<br>1° - Podemos começar a instalação da mesma forma que fizemos no desafio um, até chegar nessa tela:
<img src="https://linuxhint.com/wp-content/uploads/2020/12/image36-3.png" alt="" width="650" height="400"><br>
<br>2° - Quando chegar na etapa da imagem acima configure todas opções com ! em vermelho, inclusive crie uma senha para seu sistema e lembre-se de memoriza-la e antes de prosseguir com a instalação clique na opção 'software selection' e na parte da esquerda marque a segunda caixinha.

<img src="https://cdn.discordapp.com/attachments/468548236127502359/1163907571288125562/Screenshot_1.png?ex=65414865&is=652ed365&hm=ecb25627d4e182a1aa2bbd2588a99234bf257b278d9ea98cc6fd83ddd58c3092&" alt="" width="650" height="400"><br>


<br>3° - Após isso inicie a instalação e ao final renicie o sistema como pedido

<br>4° - Quando iniciar novemente o sistema a seguinte situação irá aparecer

<img src="https://cdn.discordapp.com/attachments/468548236127502359/1163907529433157723/carbon_3.png?ex=6541485b&is=652ed35b&hm=1675d27d1b732bb78ef59bd6449141b021f8c9ea08efbec896765167cca07510&" alt="" width="350" height="150"><br>

5° - Caso tenha criado um usuário escreva o nome e aperte Enter, caso não tenha utilize o usuário "root" e logo após coloque a senha criada na etapa 2:

<img src="https://cdn.discordapp.com/attachments/468548236127502359/1163907511464775840/carbon_4.png?ex=65414857&is=652ed357&hm=bd13ab8fb73d3b1fc189b54250943404bfa5986be21ba1a76a5add1d1ba1e60d&" alt="Primeiro contato" width="350" height="150"><br>


Pronto dessa forma o linux sem GUI está totalmente instalado, agora é necessario repetir o processo para  criar um outro servidor igual.

### Segundo passo: configurando usuarios, ips fixos
<br>1° - Caso um usuario não tenha sido criado, é preciso criar um, para isso vamos utlizar os seguintes comandos:

    sudo useradd nome-desejado
<br> Logo após precisamos configurar uma senha: 

    sudo passwd nome-desejado
<br>OBS: É importante falar que a senha deve ser confirmada duas vezes

<br>2° - Após criação de usuário precisamos verificar algumas coisas antes de prosseguimos. Primeiro devemos verificar o nome da sua interface de rede, para isso podemos utlizar o comando abaixo: 

    nmcli device status
<br> O retorno do comando deve ser parecido com essa imagem abaixo: 

<img src="https://cdn.discordapp.com/attachments/971178165479301134/1164285546944790670/image.png?ex=6542a869&is=65303369&hm=3c8dae56c7f7b8e1811f8a4d30e632d631573897a20413d23686a918e666d39f&" alt="" width="350" height="100"><br>
<br> O nome presente em verde na coluna "Device" vai ser o que iremos utilizar nas próximas vezes, caso essa linha esteja em vermelho significa que a interface está desabilitado e voce pode ativa-lo com o seguinte comando: 
            
    nmcli device connect enp0s3

<br>3° - agora podemos começar nossas configurações para configurar um IP fixo, primeiro devemos editar o arquivo onde nosso serviço está localizado dessa forma nos certificamos que esse IP ficará fixo, começamos abrindo o arquivo de configuração da nossa interface com algum editor de texto da nossa preferencia eu usarei o vi nessa parte:

    sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
<br> Dentro do arquivo devemos modificar algumas linha e adicionar outra para garantirmos que tudo funcione essas modificações são possiveis apertando a tecl i do nosso teclado, devemos adicionar o ip que queremos, nosso GATEWAY E NOSSA NETMASK os dois ultimos citados podem ser igual nos dois servers, porém o ip deve ser diferente em cada vm. Lembrando que escolhi o ip e o gateway de acordo com as configurações do meu roteador que tem como ip: 192.168.2.1 e o mesmo numero como GATEWAY por isso sempre escolherei meus ips com o seguinte formato: 192.168.2.X sendo X o numero escolhido, nesse caso escolhi o ip 192.168.2.108 como o ip da minha VM1 e 192.168.2.106 para minha VM2 vamos lá então, as seguintes linhas devem ser modificadas

    BOOTPROTO=static
<br> E as seguinte devem ser adicionadas:

    IPADDR=192.168.2.108
    NETMASK=255.255.255.0
    GATEWAY=192.168.2.1
    DNS1=8.8.8.8
    DNS2=8.8.4.4
<br> Lembrando que unica mudança da VM1 pra VM2 foi a linha IPADDR em que coloquei outro IP, existem mais linhas no arquivo mas elas não são importante pra nós no momento então depois de modificar nosso aquivo apertamos a tecla ESC e logo apos escrevemos :wq e apertamos Enter para salvar nossas mudanças e sair do arquivo

<br>3° - Agora vamos reniciar nossa network para termos certeza que tudo foi gravado devidamente, digitamos o seguinte comando em nosso terminal:

    sudo systemctl restart NetworkManager
<br>Pronto nosso ip fixo está configurado podemos ver melhor digitando ip a em nosso terminal

<img src="https://media.discordapp.net/attachments/971178165479301134/1164799334223724635/image.png?ex=654486ea&is=653211ea&hm=351a3bf93675ac16bb2989e9fade6b919ce6eccf7cbf820e33705cd2a86792d3&=" alt="" width="450" height="200">

<br>Sucesso! PPara termos 100% de certeza que tudo deu certo podemos tentar um conexão com nossa VM por meio de ssh no Sheel do windows
<br> Na barra de pesquisa do nosso computador fisico escrever PowerShell e abrimos a aplicação, quando aberto digitamos o comando ssh o nosso nome de usuario da nossa VM e o IP da mesma, segue o comando:

    ssh natangps@192.168.2.108
<br>Se tudo der certo o shell irá pedir sua senha de usuario, digite e estará controlando sua VM via ssh, segue uma imagem como exemplo

<img src="https://cdn.discordapp.com/attachments/971178165479301134/1164800548491509760/image.png?ex=6544880b&is=6532130b&hm=2f5296530f98e3b5adcee7dca02d224c26d4c31cd8a62ed8210c860a981730d9&" alt="" width="520" height="350">

<br>Nesta imagem usei alguns comandos para provar que realmente estou controlando minha VM, ou seja, de fato nossa configuração de IP está 100% funcional!

### Terceiro passo: instalando o NFS
<br> 3° - Vamos começar instalando o nfs em nossas VM'S com o seguinte comando:

    sudo dnf install nfs-utils
<br> Com a instalação realizada podemos criar uma pasta compartilhada entre nossas VMs para ver se está tudo certo, como o passo a passo será feito igualmente no nosso terceiro desafio demonstarei por lá como criar essa pasta de forma prática.


## Terceiro desafio: configurando o mariadb e o wordpress nas vms
<br> ### Primeiro passo: Ja que ja temos o NFS instalado em nossas vm's, vou começar a instalação do banco mariaDB na minha vm1 de ip(192.168.2.108)
<br> 1° - Vamos começar com o seguinte comando

    sudo dnf install mariadb-server
<br> Agora que instalamos precisamos connfigura-lo para inciar automaticamente e habilita-lo tambem, vamos fazer isso usando os comandos:

    sudo systemctl start mariadb
    sudo systemctl enable mariadb
<br> Certo agora precisamos configurar algumas coisas relacionadas a segurança do nosso banco, vamos digitar o seguinte comando em nosso console:

    sudo mysql_secure_installation
<br>  Siga as instruções que estão escritas para configurar da maneira que quiser, no meu caso não configurei usuario e nem senhas além de deixar todas as outras configurações no modo padrão (nada seguro mas bem pratico para testarmos coisas em pequena escala)
<br> Pronto nosso banco está instalado e configurado.

<br> 2° - Agora precisamos criar um banco e um usuario para que possamos nos conectar com o Wordpress futuramente, para começar vamos criar um banco primeiramente:

    mysql -u root -p
<br> Agora que estamos dentro do banco vamos criar um novo database usarei o nome 'wordpress" para meu database

    CREATE DATABASE wordpress;
<br> Pronto temos um novo database, mas precisamos criar um usuario e senha para nosso WordPress conseguir acessar e modificar o mesmo, primeiro criamos um usuario e uma senha, no meu caso o nome do usuario foi definido como 'wpress' e a senha como '1234'

    CREATE USER 'wpress'@'localhost' INDENTIFIED BY '1234';

<br> Pronto usuario criado, mas precisamos atribuir privilegios para que esse usuario possa adiministrar o banco:

    GRANT ALL PRIVILEGES ON wordpress.* TO 'wpress'@ 'localhost';
    FLUSH PRIVILEGES;
<br> Tudo certo agora precisamos criar a pasta que iremos compartilhar com nosso Wordpress para que o mesmo funcione, sendo assim vamos acessar o documento exports no seguinte diretorio /etc/exports e adicionar a pasta /mysql no compartilhamento com nossa VM2, primeiro acessamos o documento exports com o editor de texto da sua preferencia, vou utilizar o nano para isso:

    sudo nano /etc/exports
<br> Se tiver alguma linha no arquivo apague e escreva uma nova compartilhando a pasta do diretorio /var/lib/mysql com nossa VM2:

    /var/lib/mysql 192.168.2.106(rw,sync,no_root_squash)
<br> Salve o arquivo apertando Ctrl + O e saia utilizando Ctrl + X
<br> Agora reniciamos o NFS para garantir que nossas mudanças sejam aplicadas:

    sudo systemctl restart nfs
<br>Perfeito! Da parte da nossa nossa VM1 tudo está configurado vamos para VM2 agora terminar as configurações e instalar nosso Wordpress

### Segundo passo: instalando e configurando nosso wordpress
<br> 1° - Vamos primeiro terminar de configurar nossa pasta compartilhada com nossa VM1 para isso precisamos criar um pasta nessa VM2 que será nossa pasta compartilhada que usaremos em nosso wordpress:

    sudo nkdir /mnt/mariadb_shared_vm2
<br> Agora com nossa pasta criada vamos montar finalmente nosso NFS com o seguinte comando:

    sudo mount -t nfs 192.168.2.106:/var/lib/mysql /mnt/maria_db_shared_vm2
<br> Pronto Tudo está conectado agora, vamos nos preparar para instalar nosso wordpress agora, primeiro devemos instalar as dependencias para melhor compatibilidade dele, vamos então executar os comandos abaixo para instalar o Apache e o PHP em nossa maquina


    sudo dnf install php php-mysqlnd php-gd php-xml httpd
    sudo dnf install epel-release
    sudo dnf install http://rpms.remirepo.net/enterprise/remi-release-8.rpm
    sudo dnf install dnf-utils
    sudo dnf module enable php:remi-7.4
    sudo dnf install php
    sudo dnf install php-{cli,mysqlnd,xml,pdo,gd,mbstring,json}
<br> Com tudo devidamente instalado precisamos habilitar e inicar nosso Apache com os seguintes comandos:

    sudo systemctl start httpd
    sudo systemctl enable httpd
<br> Pronto, agora sim podemos instalar nosso wordpress mas antes vamos ao diretorio em que vamos instalar ele para deixarmos o ambiente mais organizado

    cd /var/www/html
<br>2° - Uma vez nessa pasta podemos comecar a instalação vamos lá:

    wget https://wordpress.org/latest.tar.gz
    tar -xvf latest.tar.gz
<br> Agora entramos na pasta com o comando:

    cd wordpress/
<br> Uma vez dentro da pasta abrimos o arquivo  de configuracao 'wp-config.php' pode ser que ele estaja com um nome a mais depois do config então para ter certeza do nome completo digite ls no terminal e veja qual o nome correto como no print abaixo

<img src="https://cdn.discordapp.com/attachments/971178165479301134/1164788576828014612/image.png?ex=65447ce5&is=653207e5&hm=ab82490bcc4d17dba22ec54c6a20fd312d65b10fc514e5951a87f8509e760340&" alt="" width="600" height="200">

<br> No meu caso o nome está como 'wp-config-sample.php' vamos abri-lo usando o editor de texto nano

    sudo nano wp-config-sample.php
<br> Uma vez dentro do arquivo precisamos modicar algumas informações alem de colocar como database nossa pasta compartilhada do server 1 que está com nosso mariaDB
sendo assim vamos modificas nosso arquivo assim:

<br> Antes da modificação:

    define('DB_NAME', 'database name');
    define('DB_USER', 'dbusername');
    define('DB_PASSWORD', 'dbpassword');
    define('DB_HOST', 'localhost');

<br> Depois de preencher as informações: 

    define('DB_NAME', '/mnt/mariadb_share_vm2/wordpress');
    define('DB_USER', 'wpress');
    define('DB_PASSWORD', '1234');
    define('DB_HOST', 'localhost');
<br>É importante dizer que não é somente essas linhas que estão em nosso arquivos mas elas são a mais importante pro nosso funcionamento
<br> Agora podemos salvar nosso arquivo com Ctrl + O e sair com Ctrl + X

<br> Pronto! Se configuramos certo basta apenas abrir um navegador em nosso computador fisico e digitar na barra de pesquisa: http://192.168.2.106/wordpress/ devemos ver a seguinte tela:

<img src="https://cdn.discordapp.com/attachments/971178165479301134/1164790701570134047/image.png?ex=65447ee0&is=653209e0&hm=9a3ad005de7af81c8e050f486815d07f39ee79aaf7f69cafcd15d5ee68a67aea&" alt="" width="650" height="250">



    

    


    

  


