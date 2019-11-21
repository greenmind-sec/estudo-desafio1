
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

Podemos inserir o email ou se cadastrar com uma conta google. Vou inserir meu email **greenmind.sec@gmail.com** e clicar em **Verify My Email Address**.
![](images/05.png)

Vamos ver no email a confirmação, vamos clicar no link que recebemos em seguida..
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


#### Tomcat
> https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-centos-7

#### Wordpress
> https://linuxize.com/post/install-mysql-on-centos-7/


sudo yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm


oKho-swAu4:h

ALTER USER 'root'@'localhost' IDENTIFIED BY 'Senhatemporaria@12345';

```sh
create user magentouser@localhost identified by 'Magento123@';
grant all privileges on magentodb.* to magentouser@localhost identified by 'Magento123@';
flush privileges;
```

https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-centos-7


sudo yum install php70w-common --skip-broken

```sh
# Redirect HTTP -> HTTPS
server {
    listen 80;
    server_name blog-juliocesar.juliocesar.tk;

    #include snippets/letsencrypt.conf;
    #return 301 https://example.com$request_uri;
}

# Redirect WWW -> NON WWW
server {
    #listen 443 ssl http2;
    listen 80;
    server_name blog-juliocesar.juliocesar.tk;

    #ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    #ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    #ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem;
    #include snippets/ssl.conf;

    #return 301 https://example.com$request_uri;
}

server {
    #listen 443 ssl http2;
    server_name blog-juliocesar.juliocesar.tk;

    root /var/www/html/blog;
    index index.php;

    # SSL parameters
    #ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    #ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    #ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem;
    #include snippets/ssl.conf;
    #include snippets/letsencrypt.conf;

    # log files
    #access_log /var/log/nginx/example.com.access.log;
    #error_log /var/log/nginx/example.com.error.log;

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
        fastcgi_pass unix:/run/php-fpm/www.sock;
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


#### Magento
https://www.howtoforge.com/tutorial/how-to-install-magento-2-1-on-centos-7/

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

> https://stackoverflow.com/questions/23948527/13-permission-denied-while-connecting-to-upstreamnginx
