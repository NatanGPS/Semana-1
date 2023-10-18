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

<br>4° - Quando inicar novemente o sistema a seguinte situação irá aparecer

<img src="https://cdn.discordapp.com/attachments/468548236127502359/1163907529433157723/carbon_3.png?ex=6541485b&is=652ed35b&hm=1675d27d1b732bb78ef59bd6449141b021f8c9ea08efbec896765167cca07510&" alt="" width="350" height="150"><br>

5° - Caso tenha criado um usuário escreva o nome e aperte Enter, caso não tenha utilize o usuário "root" e logo após coloque a senha criada na etapa 2:

<img src="https://cdn.discordapp.com/attachments/468548236127502359/1163907511464775840/carbon_4.png?ex=65414857&is=652ed357&hm=bd13ab8fb73d3b1fc189b54250943404bfa5986be21ba1a76a5add1d1ba1e60d&" alt="Primeiro contato" width="350" height="150"><br>


Pronto dessa forma o linux sem GUI está totalmente instalado, agora é necessario repetir o processo para  criar um outro servidor igual.

### Segundo passo: configurando usuarios, ips fixos e NFS
<br>1° - Caso um usuario não tenha sido criado, é preciso criar um, para isso vamos utlizar os seguintes comandos:

    sudo useradd nome-desejado
<br> Logo após precisamos configurar uma senha: 

    sudo passwd nome-desejado
<br>OBS: É importante falar que a senha deve ser confirmada duas vezes

<br>2° - Após criação de usuário precisamos verificar algumas coisas antes de prosseguimos. Primeiro devemos verificar o nome do seu serviço de rede, para isso podemos utlizar o comando abaixo: 

    nmcli device status
<br> O retorno do comando deve ser parecido com essa imagem abaixo: 

<img src="https://cdn.discordapp.com/attachments/971178165479301134/1164285546944790670/image.png?ex=6542a869&is=65303369&hm=3c8dae56c7f7b8e1811f8a4d30e632d631573897a20413d23686a918e666d39f&" alt="" width="350" height="100"><br>
<br> O nome presente em verde na coluna "Device" vai ser o que iremos utilizar nas próximas vezes, caso essa linha esteja em vermelho significa que o serviço está desabilitado e voce pode ativa-lo com o seguinte comando: 
            
    nmcli device connect nome-serviço

<br>3° - agora podemos começar nossas configurações para configurar um IP fixo, primeiro devemos editar o arquivo onde nosso serviço está localizado dessa forma nos certificamos que esse IP ficará fixo, começamos pedindo acesso root ao linux:

    sudo su
<br> o sistema irá pedir sua senha
<br> Após isso precisamos abrir o arquivo com algum editor de texto para edita-lo, escolhi o vi:

    sudo vi /etc/sysconfig/network-scripts/ifcfg-nome-servico
<br> Uma vez que estamos dentro do arquivo apertamos a tecla "i" para editar o arquivo. É importante falar que vamos utilizar os seguinte IP para a máquina 1: 192.168.1.10 e para a segunda 192.168.1.11, tirando os IP´s todas as outras configurações podem ser compartilhadas na hora de editar nosso arquivo. Segue os atributos que devemos mudar no arquivo, lembrando que aqueles que não existem podem ser criados. Os demais atributos não precisam ser mudados

    BOOTPROTO=static
    IPADDR=192.168.1.10  #deve ser unico a cada servidor
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    DNS1=8.8.8.8
    DNS2=8.8.4.4
<br> Após mudar os atributos como acima, apertamos Esc e logo após :wq e apertamos Enter pra salvar e sair do arquivo


