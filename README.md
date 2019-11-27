
## Criação de dominio
Vamos usar o projeto
```sh
http://www.dot.tk/pt/index.html
```

![](images/01.png)

Podemos ver que está disponivel nosso endereço
```sh
juliocesar.tk
```

Vamos clicar em **Obtê-lo agora!**.

Em seguida podemos ir em **Pagamento**.

![](images/02.png)

Depois de clicar em pagamento vamos ser redirecionandos para

![](images/03.png)

Em seguida vamos clicar em **Continue** e vamos ser redirecionandos para a seguinte pagina.

![](images/04.png)

Podemos inserir o email ou se cadastrar com uma conta google. Vou inserir meu email greenmind.sec@gmail.com e clicar em **Verify My Email Address**.

![](images/05.png)

Vamos ver no email a confirmação, vamos clicar no link que recebemos em seguida.

![](images/06.png)

Depois vamos ser redirecionandos para a pagina de cadastro, vamos precisar inserir algumas informações, como:

- Nome
- Sobrenome
- Nome da empresa
- Endereço 1

![](images/07.png)

Alem disso precisamos colocar
- Codigo postal
- Cidade
- Pais
- Estado
- Numero de telefone
- Email
- Senha
- Confirmar senha

![](images/08.png)

> Não podemos esquecer de aceitar **I have read and agree to the Terms & Conditions**.
![](images/09.png)

Em seguida vamos clicar em **Complete Order**.

Se der tudo certo vamos ser redirecionandos para a pagina **Order Confirmation**.
![](images/10.png)
> Vamos clicar em **Click here to go to your Client Area** para sermos redirecionandos para a Area do cliente.

Vamos ser redirecionandos para a pagina de login
![](images/11.png)

Vamos ser redirecionandos para a pagina de login com o DNS padrão.
![](images/12.png)

## Configurando Route 53
> https://console.aws.amazon.com/route53/

![](images/13.png)

Assim que clicar em **Criar zona hospedada** vai ser aberto uma aba do lado direito.

![](images/14.png)
> Em seguida vamos clicar em **Criar**.

Vai ser retornado alguns DNS que vamos adicionar ao dominio
![](images/15.png)

## Adicionando meu DNS

![](images/16.png)

Em seguida vamos em **Manage Domain**.

![](images/17.png)

Em seguida vamos até **Management Tools**.

![](images/18.png)

Vamos agora mudar para **Use custom nameservers (enter below)**.

![](images/19.png)

Vamos adicionar os DNS da amazon

![](images/20.png)

Vai ficar Assim

![](images/21.png)

Vai ficar assim

![](images/22.png)

## Criando servidor
Foi pedido um servidor **CentOS7**, vamos usar a imagem.

![](images/23.png)

Vamos clicar em **Continuar**.

![](images/24.png)

Vamos selecionar **t2.micro**.

![](images/25.png)

Vamos ver os detalhes das instancias

![](images/26.png)

Vamos colocar 30 GB de HD

![](images/27.png)

Vamos criar uma tag para ele

![](images/28.png)

Vamos ver as regras disponiveis

![](images/29.png)

Vamos revisar a instancia

![](images/30.png)

Vamos criar uma nova key

![](images/31.png)
> Vamos realizar o download dela

Em seguida só precisamos clicar em **Launch instances**.

### Acessando maquina
Com a chave em mãos vamos precisar dar pemissão a chave e por usarmos o centos vamos usar o usuario **centos**.

Vamos no diretorio da chave dar permissão **400** usando o chmod.
```sh
chmod 400 juliocesar-desafio1.pem
```

Antes de ter acesso a maquina

![](images/32.png)

Deu tudo certo, agora vamos configurar um Elastic IP.

### Configurando Elastic IP
Vamos ir até o menu ao lado esquerdo no menu **Elastic IPs**.

![](images/33.png)

Alocando novo IP

![](images/34.png)
> Vamos clicar em **Allocate**.

Vai ser gerado um novo IP

![](images/35.png)

Vamos agora **Associate**.

![](images/36.png)

Podemos acessar usando
```sh
ssh -i "juliocesar-desafio1.pem" centos@ec2-3-213-226-214.compute-1.amazonaws.com
```

### Configurando infraestrutura
Vamos alterar para root.
```sh
sudo su
```

Vamos atualizar o sistema
```sh
yum update
```

Vamos instalar o **epel-release**.
```sh
yum -y install epel-release
```

Vamos instalar o nginx
```sh
yum -y install nginx
```

Vamos iniciar ele usando
```sh
systemctl start nginx
```

Em seguida vamos adicionar o nginx
```sh
systemctl enable nginx
```

Instalando o nano
```sh
yum install nano
```

Com tudo vamos entrar no nosso IP e vamos ter o retorno.
![](images/37.png)

#### Site
Para o desafio precisamos criar os projetos em uma ordem de diretorios da seguinte forma.
```sh
[centos@ip-172-31-24-40 html]$ pwd
/var/www/html
```

Vamos criar um diretorio chamado **site**.
```sh
mkdir site
```
> ele vai ficar no endereço **/var/www/html/site**.

Vamos agora criar um site html simples, da seguinte forma:
```sh
echo "<center><h1>Consegui chegar</h1><center>" > /var/www/html/index.html
```

Agora vamos criar um arquivo de configuração para o **nginx**.

Vamos até o diretorio
```sh
cd /etc/nginx/conf.d/
```

Vamos criar agora o arquivo chamado **site.conf**.
```sh
server {
   listen  80;
   server_name site-juliocesar.juliocesar.tk;
   error_log /var/log/nginx/blog/error.log;
   location / {
       root  /var/www/html/site/;
       index  index.html index.htm;
   }
   error_page  500 502 503 504  /50x.html;
}
```

Podemos testar ver se o arquivo está ok usando
```sh
nginx -t
```

Vamos agora resetar o serviço
```sh
service nginx restart
```

Vamos agora adicionar na **route 53** o apontamento.
> Nesse exemplo meu IP é o **3.213.226.214**.

![](images/conf-route53-site.png)

#### Tomcat
> https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-centos-7

Inicialmente vamos instalar o **OpenJDK**.
```sh
sudo yum install java-1.7.0-openjdk-devel
```

Vamos criar um usuario para o tomcat
```sh
sudo groupadd tomcat
```

Vamos precisar instalar o **wget**.
```sh
sudo yum install wget
```

Vamos realizar o download do tomcat usando o wget no seguinte link
```sh
wget https://www.apache.org/dist/tomcat/tomcat-8/v8.5.49/bin/apache-tomcat-8.5.49.tar.gz /tmp
```

Agora vamos descompactar.
```sh
cd /tmp
tar xf apache-tomcat-8.5.49.tar.gz
mv apache-tomcat-8.5.49 /opt/tomcat
```

Agora vamos alterar a permissão do arquivo
```sh
sudo useradd -M -s /bin/nologin -g tomcat -d /opt/tomcat tomcat
```

Agora vamos conceder ao grupo do tomcat a propriedade de todo o diretorio da instalação.
```sh
sudo chgrp -R tomcat /opt/tomcat
```

Em seguida vamos fornecer ao grupo tomcat acesso de leitura ao diretório conf e todo o seu conteúdo e execute o acesso ao próprio diretório:
```sh
sudo chmod -R g+r conf
sudo chmod g+x conf
```

Em seguida, vamos tornar o usuário do tomcat o proprietário dos diretórios webapps, work, temp e logs:
```sh
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

Vamos criar um arquivo de inicialização.
```sh
sudo vi /etc/systemd/system/tomcat.service
```

O valor do arquivo é o seguinte
```sh
# Systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

Não podemos esquecer de salvar e sair.

Vamos realizar um reload do systemd
```sh
sudo systemctl daemon-reload
```

Vamos dar um start no serviço tomcat
```sh
sudo systemctl start tomcat
```

Vamos ver o status do tomcat
```sh
sudo systemctl status tomcat
```

Se você deseja ativar o serviço Tomcat, para que ele seja iniciado na inicialização do servidor, execute este comando:
```sh
sudo systemctl enable tomcat
```

> Agora nosso tomcat já vai estar funcionando na porta **8080**, para testar na amazon vamos precisar realizar a adição da porta 8080 no Security groups.

Nesse caso vamos criar um proxy reverso no diretorio **/etc/nginx/conf.d/site.conf**. O valor do arquivo é o seguinte.
```sh
server {
 listen 80;
 server_name tomcat-juliocesar.juliocesar.tk;
 access_log /var/log/nginx/tomcat/access.log;
 error_log /var/log/nginx/tomcat/error.log;


 location / {
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header Host $host;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  proxy_pass http://127.0.0.1:8080;
 }
}
```
> Não esqueça de salvar

Em seguida vamos dar um checar o nginx e dar um restart no serviço.
```sh
sudo nginx -t
sudo service nginx restart
```

Podemos realizar o restart no tomcat.
```sh
sudo service tomcat restart
```

#### Mysql
Vamos adicionar o mysql no repositorio do centos
```sh
sudo yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
```

Vamos atualizar o sistema
```sh
sudo yum update
```

Instalando o mysql
```sh
sudo yum install mysql-community-server
```

Vamos adicionar o mysql para iniciar junto com o sistema
```sh
sudo systemctl enable mysqld
```

Vamos iniciar o mysql
```sh
sudo systemctl start mysqld
```

Agora vamos analisar qual é a senha de instalação do mysql.
```sh
sudo grep 'temporary password' /var/log/mysqld.log
```

Vamos ter uma mensagem semelhante a
```sh
A temporary password is generated for root@localhost: q&0)V!?fjksL
```

É recomendado usar as configurações para uma instalação segura.
```sh
sudo mysql_secure_installation
```



#### Wordpress
> https://linuxize.com/post/install-mysql-on-centos-7/


##### Criando banco de dados
Vamos criar um banco chamado **wordpress**.
```sh
CREATE DATABASE wordpress;
```

Vamos criar um usuario para o banco com o nome **wordpressuser** e a senha **senhatemporaria**.
```sh
CREATE USER wordpressuser@localhost IDENTIFIED BY 'senhatemporaria';
```

Vamos conceder privilegio para o usuario que criamos anteriomente
```sh
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'senhatemporaria';
```

Vamos realizar as modificações usando
```sh
FLUSH PRIVILEGES;
```

Em seguida podemos sair usando
```sh
exit
```


##### Instalando wordpress
Vamos ate o diretorio
```sh
cd /var/www
```

Vamos criar um diretorio chamado blog
```sh
mkdir html
```

Vamos iniciar instalando o **php-gd**.
```sh
sudo yum install php-gd
```

Vamos até o diretorio /tmp e la realizar o download do wordpress.
```sh
cd /tmp
```

Vamos realizar o download dele
```sh
wget http://wordpress.org/latest.tar.gz
```

Vamos descompactar e mandar ele para /var/www/html
```sh
tar xf latest.tar.gz
```

Vamos agora mover o diretorio para outro diretorio
```sh
mv wordpress /var/www/html/blog
```

Vamos criar o diretorio uploads
```sh
mkdir /var/www/html/blog/wp-content/uploads
```

Vamos alterar a permissão do diretorio do blog
```sh
sudo chown -R nginx:nginx /var/www/html/blog/*
```

Vamos ir até o diretorio do blog e fazer uma copia do arquivo de configuração chamado **wp-config.php**.
```sh
cp wp-config-sample.php wp-config.php
```

Vamos alterar o arquivo **wp-config.php**.
```sh
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');
```
> Nesse passo vamos setar as senhas do nosso banco.

Vamos agora criar um arquivo de configuração em **/etc/nginx/conf.d/blog.conf** com o seguinte valor.
```sh
server {
server_name blog-juliocesar.juliocesar.tk;
   root /var/www/html/blog/;
   index index.html index.htm index.php;
   #access_log /var/log/nginx/blog/access.log;
   #error_log /var/log/nginx/blog/error.log;
 location = /favicon.ico {
       log_not_found off;
       access_log off;
   }
   location = /robots.txt {
       allow all;
       log_not_found off;
       access_log off;
   }
   location / {
       try_files $uri $uri/ /index.php?$args;
   }
   location ~ \.php$ {
       try_files $uri =404;
       fastcgi_pass unix:/run/php/php-fpm.sock;
       fastcgi_index   index.php;
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       include fastcgi_params;
   }
   location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
       expires max;
       log_not_found off;
   }
}
```

Vamos realizar uma configuração no dirtorio /var/www/html/blog para conceder permissão ao sistema.
```sh
sudo setsebool -P httpd_can_network_connect on
getenforce
chcon -Rt httpd_sys_content_t /var/www/html/blog
```

Vamos agora ver se o nosso arquivo de configuração está OK.
```sh
sudo nginx -t
```

Agora vamos realizar um restart no serviço nginx.
```sh
sudo service nginx restart
```

#### Magento
> https://www.howtoforge.com/tutorial/how-to-install-magento-2-1-on-centos-7/

Vamos adicionar PHP-FPM ao repositorio.
```sh
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

Vamos instalar o PHP-FPM
```sh
yum -y install php70w-fpm php70w-mcrypt php70w-curl php70w-cli php70w-mysql php70w-gd php70w-xsl php70w-json php70w-intl php70w-pear php70w-devel php70w-mbstring php70w-zip php70w-soap
```

Vamos abrir o **/etc/php.ini** e inserir algumas configurações.
```sh
nano /etc/php.ini
```

Vamos precisar configurar o **php.ini**.
> Vamos descomentar a linha
```sh
cgi.fix_pathinfo=0
```

> Vamos realizar a configuração e alterar os valores
```sh
memory_limit = 512M
max_execution_time = 1800
zlib.output_compression = On
```

> Vamos descomentar a seguinte linha
```sh
session.save_path = "/var/lib/php/session"
```
> Não podemos de salvar e sair.


Agora vamos alterar o arquivo **/etc/php-fpm.d/www.conf**.
> Vamos agora setar o socket para o **php-fpm**.
```sh
listen = /var/run/php/php-fpm.sock
```

> Vamos adicionar o grupo, usuario nginx e a permissão.
```sh
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

> Para finalizar vamos descomentar as seguintes linhas
```sh
env[HOSTNAME] = $HOSTNAME
env[PATH] = /usr/local/bin:/usr/bin:/bin
env[TMP] = /tmp
env[TMPDIR] = /tmp
env[TEMP] = /tmp
```
> Não esqueça de salvar e sair

Vamos agora criar dois diretorios
```sh
mkdir -p /var/lib/php/session/
chown -R nginx:nginx /var/lib/php/session/
```

Criando novo diretorio para o php-fpm
```sh
mkdir -p /run/php/
chown -R nginx:nginx /run/php/
```

Vamos agora iniciar o php-fpm e inserir ele na inicialização do sistema.
```sh
systemctl start php-fpm
systemctl enable php-fpm
```

Vamos verificar se tem algum erro e analisar onde está o **php-fpm.sock**.
```sh
netstat -pl | grep php-fpm.sock
```

Vamos agora criar o banco para o projeto.
```sh
create database magentodb;
create user magentouser@localhost identified by 'Magento123@';
grant all privileges on magentodb.* to magentouser@localhost identified by 'Magento123@';
flush privileges;
```

Vamos agora ir até o diretorio do magento e realizar o a instalação do **composer**.
```sh
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/bin --filename=composer
```

Depois da instalação completa vamos realizar um
```sh
composer -v
```

Agora vamos obter o **magento**.
```sh
cd /var/www/
mkdir html
cd html
wget https://github.com/magento/magento2/archive/2.1.zip
```

Caso não tenha o **unzip** podemos instalar ele
```sh
yum -y install unzip
```

Vamos descompactar e renomear ele
```sh
unzip 2.1.zip
mv magento2-2.1 magento2
```

Vamos até o diretorio do magento e realizar a instalação das dependencias
```sh
cd magento2
composer install -v
```

Vamos agora até o diretorio de instalação
```sh
cd /var/www/html/magento2
```

Vamos ver se os valores são os que vamos usar e realizar o setup.
```sh
bin/magento setup:install --backend-frontname="adminlogin" \
--key="biY8vdWx4w8KV5Q59380Fejy36l6ssUb" \
--db-host="localhost" \
--db-name="magentodb" \
--db-user="magentouser" \
--db-password="Magento123@" \
--language="en_US" \
--currency="USD" \
--timezone="America/Sao_Paulo" \
--use-rewrites=1 \
--use-secure=0 \
--base-url="http://loja-juliocesar.juliocesar.tk" \
--base-url-secure="http://loja-juliocesar.juliocesar.tk" \
--admin-user=adminuser \
--admin-password=admin123@ \
--admin-email=admin@newmagento.com \
--admin-firstname=admin \
--admin-lastname=user \
--cleanup-database
```

Vamos agora realizar a modificação de permissão
```sh
chmod 700 /var/www/magento2/app/etc
chown -R nginx:nginx /var/www/magento2
```


> https://stackoverflow.com/questions/23948527/13-permission-denied-while-connecting-to-upstreamnginx


O arquivo de confoguração do nginx é o seguinte e ele precisa estar em **/etc/nginx/conf.d**.

Vamos criar o arquivo **magento.conf**.
```sh
upstream fastcgi_backend {
        server  unix:/run/php/php-fpm.sock;
}

server {

        listen 80;
        server_name loja-juliocesar.juliocesar.tk;
        set $MAGE_ROOT /var/www/html/magento2;
        set $MAGE_MODE developer;
        include /var/www/html/magento2/nginx.conf.sample;
}
```

Vamos ver se está tudo OK com o arquivo de configuração
```sh
nginx -t
systemctl restart nginx
```
