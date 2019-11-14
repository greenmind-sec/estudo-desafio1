
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
