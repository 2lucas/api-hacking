# api-hacking



#### Instalação do Docker | Ubuntu
1. sudo apt install gnome-terminal
2. sudo apt install apt-transport-https ca-certificates curl software-properties-common
3. sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
4. sudo apt update
5. sudo apt install docker-ce
6. sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#### Instalação do Owasp Juice Shop
1. docker pull bkimminich/juice-shop
2. docker run --rm -p 3000:3000 bkimminich/juice-shop
3. Então basta acessar http://localhost:3000/


#### Instalação do VAMPI
1. docker pull erev0s/vampi
2. docker run -p 5000:5000 erev0s/vampi:latest
3. Então basta acessar http://127.0.0.1:5000


#### Instalação do Postman
1. Visite https://www.postman.com/downloads/
2. Em caso de Windows, faça a instalação normalmente selecionando o Windows
3. Em caso de Linux, selecione a arquitetura do seu sistema e baixe o .tar.gz
4. Extraia o .tar.gz e de dois cliques no executavel `"Postman"`
5. Cadastre ou faça login na sua conta
6. Se não sair da tela de Login mesmo após efetuar a autenticação, feche o Postman e execute o via terminal com `"./Postman"` dentro do diretório onde você extraiu o conteúdo do .tar.gz
7. Faça o login novamente

##### Como configurar o Burp
- Adicionando o scopo do proxy
![Pasted image 20240422144433](https://github.com/2lucas/api-hacking/assets/82060275/56f08498-0c85-4c11-a335-adc9a437658e)

- Regras de intercepção para focar apenas aquilo que está dentro do escopo
![Pasted image 20240419144503](https://github.com/2lucas/api-hacking/assets/82060275/0e6b09eb-750a-4d3e-8115-e5320dcd6e41)

- Usaremos o browser do Burp Suite
![Pasted image 20240419144837](https://github.com/2lucas/api-hacking/assets/82060275/6f782f50-37e3-4c73-85ed-a7472da28bcd)


##### Os primeiros passos para um pentest API
- Busque sempre fazer o fluxo feliz, interaja com a aplicação normalmente, como um usuário que não tem malicia, mas analise os potenciais pontos de exploração. Como endpoints sensíveis, parâmetros presentes, faça uma anotação ou memorize esses locais, tornando eles como prioridade para iniciar o ataque.

- **Observe algumas possíveis vulns que podem ter navegando no meu lateral esquerdo e narre elas**
- Nessa tela anote o dominio do e-mail para fazer um brute-force na etapa de login 
![Pasted image 20240419150303](https://github.com/2lucas/api-hacking/assets/82060275/bbae8cae-ded3-4a2b-997e-bca048e4d742)




- Ao identificar uma API no tráfego interceptado, o Nuclei pode entrar em ação para descobrir o Swagger e colaborar para mapear uma superfície de ataque ainda maior.
	- Exemplo identificando uma API
		![Pasted image 20240419134527](https://github.com/2lucas/api-hacking/assets/82060275/b601d739-462b-4b99-9da0-69e302d5746a)

	- Yaml responsável por descobrir os swaggers https://gist.github.com/emadshanab/d3359d3c8d2dd11d77b240d6dab61733 baixe localmente e execute da seguinte forma
		![Pasted image 20240419134548](https://github.com/2lucas/api-hacking/assets/82060275/bb439a4d-81e1-4475-bbc0-568a4182ec80)



##### Como descobrir possíveis usuários?
- Através da vulnerabilidade de **User Enumeration** / Enumeração de usuários
- O campo de cadastro e esqueci minha senha podem revelar se um usuário existe ou não dentro da aplicação e através disso, podemos descobrir usuários que podem se tornar potenciais alvos durante o pentest.
	Vá até o "Esqueci minha Senha"
	![Pasted image 20240419143241](https://github.com/2lucas/api-hacking/assets/82060275/40a5f089-1265-479e-b3da-3be5d736cf90)



- Identifique a requisição que verifica se o e-mail existe
	![Pasted image 20240419150940](https://github.com/2lucas/api-hacking/assets/82060275/8f9cb9e4-9e52-4dca-983a-d9aff1b90702)



- Mande para o intruder e defina o payload 
	![Pasted image 20240419151127](https://github.com/2lucas/api-hacking/assets/82060275/46dc8459-29ef-421a-b6d0-90effdb48713)



- Selecione a própria lista de 'Usernames' que o Burp Pro disponibiliza 
	![Pasted image 20240419152125](https://github.com/2lucas/api-hacking/assets/82060275/a844a927-6184-463e-8f5c-f63eb863e8fc)



- Usernames descobertos
  ![Pasted image 20240419152344](https://github.com/2lucas/api-hacking/assets/82060275/8d46d214-dd5e-438c-b208-6bb368daca15)


#### Password brute-force
> Reutilize o e-mail de admin para a tela de login e senha

Mande para o intruder
![Pasted image 20240419153310](https://github.com/2lucas/api-hacking/assets/82060275/cb320905-d809-4dd4-a5b7-f41cf3f600ce)


Adicione o payload no campo de password
![Pasted image 20240419153338](https://github.com/2lucas/api-hacking/assets/82060275/6fa5617e-bf50-4ad8-9e9e-1b9494eb1d32)


Selecione a wordlist de senhas disponibilizada no repositório do seclists
- Cole o conteudo da wordlist em um arquivo
https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt
![Pasted image 20240419153647](https://github.com/2lucas/api-hacking/assets/82060275/b8a0ff5a-63d9-4124-9836-1a49bb88e360)


No burp suite selecione o arquivo da wordlist que você criou localmente (Clicando em Load)
![Pasted image 20240419153908](https://github.com/2lucas/api-hacking/assets/82060275/4b1d5b41-bafc-4ef2-a3db-7f8594be4976)

![Pasted image 20240419154256](https://github.com/2lucas/api-hacking/assets/82060275/74bbacc7-0b2a-4d6f-b25b-f684e8bb0da2)



#### VAmPI
Visite o arquivo  `VAmPI.postman_collection.json` dentro de openapi_specs no repo https://github.com/erev0s/VAmPI
![Pasted image 20240422094219](https://github.com/2lucas/api-hacking/assets/82060275/f25e4bc4-a598-4dd0-b86d-28f35b85caa5)


Salve o conteudo da colletion em um json localmente
![Pasted image 20240422095218](https://github.com/2lucas/api-hacking/assets/82060275/b2b89341-4b2c-4f40-9e4f-920d06db2142)


Abra com o postman e importe a collection usando CTRL + O
![Pasted image 20240422102117](https://github.com/2lucas/api-hacking/assets/82060275/d234eb38-8f57-4655-908c-05dea5e83afa)



Para jogar o tráfego do Postman para o Burp, precisamos usar o Postman com Proxy, para isso, basta ir para -> Configurações (na engrenagem) -> Proxy Server e deixe essas configurações
![Pasted image 20240422105024](https://github.com/2lucas/api-hacking/assets/82060275/8ecf922e-9b8e-421b-9f72-97d396256bcc)



#### Pentest - Broken Authentication
Algumas vezes tendo o swagger em mãos, podemos consumir determinados endpoints sensíveis estando autenticados. Mas e se removermos o header responsável pela nossa autenticação? Isso leva para uma vulnerabilidade de Broken Authentication

Pela documentação sabemos que o endpoint `/users/v1` **“ Retrive all users”**, então vamos ver o que isso significa.
![Pasted image 20240422112554](https://github.com/2lucas/api-hacking/assets/82060275/e66dca4d-3c33-4813-bca2-213996e5c86a)

Essa é a nossa primeira vulnerabilidade que acontece decorrente de um Broken Authentication, tendo como impacto o vazamento de informações, onde o criminoso pode vender essas informações ou fazer o reuso das mesmas durante o pentest.
  
 Não tem sentido ter esse endpoint disponibilizado externamente, ainda mais sem autenticação. Dependendo da quantidade usuários, os recursos podem entrar em exaustão causando Denial of Service. 

#### Pentest - Broken Object Level Authorization

O swagger indica que existe um endpoint de depuração chamado `/users/v1/_debug`  **Retrieves all details for all users (debug)**, a depuração em ambientes de produção pode revelar todos os tipos de dados, então vamos dar uma olhada.

![Pasted image 20240422135422](https://github.com/2lucas/api-hacking/assets/82060275/9f23efe5-0ca1-4493-8ab4-8af94e18ee6d)

Para ver se esses dados são realmente válidos, vamos ver se conseguimos fazer login usando os dados de admin que foram retornados.  
Ao enviar uma requisição POST para o `/users/v1/login` com os dados de login do administrador que foram descobertos, a autenticação deve ocorrer.

-> Para isso basta usar o Repeater do Burp para enviar requisições repetidamente.
Requisição padrão
![Pasted image 20240422135812](https://github.com/2lucas/api-hacking/assets/82060275/77d09ac1-5e69-4361-ad14-cc2a54983aa9)


Requisição de autenticação
![Pasted image 20240422135856](https://github.com/2lucas/api-hacking/assets/82060275/9c6b224a-9fed-44ec-b333-14ab727b4ef6)



- Essa falha acontece devido a ausência de autorização para consumir esse endpoint sensível para o contexto da aplicação, levando a uma falha de Broken Object Level Authorization


#### Pentest - Escalonamento de privilégios
Quando olhamos para o endpoint `/users/v1/_debug` notamos que existe uma propriedade chamada “admin” que é definida como true para usuários que são admin no sistema. Agora, iremos tentar cadastrar um novo usuário e enviar essa propriedade junto com a requisição de cadastro.

Vamos enviar o seguinte para `/users/v1/register` o **Repeater** do burp e colocar a propriedade `admin: true`. 
![Pasted image 20240422143925](https://github.com/2lucas/api-hacking/assets/82060275/1460124c-a453-45b0-8b83-457f11b6d7e6)


Consumindo novamente o endpoint de debug para verificar a role do usuário cadastrado, que agora é admin
![Pasted image 20240422144433](https://github.com/2lucas/api-hacking/assets/82060275/d6e09a1d-0240-4816-ac68-d8cbf271a519)




#### Pentest | Autenticado - Insecure Direct Object Reference
- Observe que existe `/books/v1` **Retrive all books** para retornar todos os livros presentes 
![Pasted image 20240422193754](https://github.com/2lucas/api-hacking/assets/82060275/7055a3fa-098e-4e73-b005-736e18ac6a04)


- Tente solicitar os dados de um unico livro específico **Retrieves book by title along with secret** `/books/v1/:book_title` e indique o valor de um livro.
![Pasted image 20240422194206](https://github.com/2lucas/api-hacking/assets/82060275/562b8529-b33a-4e6a-a0a2-b611377c7077)



- Ok, agora faça o Login em `/users/v1/login` copie o conteúdo da propriedade **"auth_token"** e vá para um endpoint que requer autenticação, como **Retrieves book by title along with secret** `/books/v1/:book_title` (envie para o **repeater**)
- Adicione o header `Authorization: Bearer *TOKEN_RECEBIDO_ANTERIORMENTE*`
![Pasted image 20240422150501](https://github.com/2lucas/api-hacking/assets/82060275/6175a37d-40a3-47e0-bb0f-9e9f9f5340ac)


 Teremos acesso indevido ao conteudo de um livro
![Pasted image 20240422194535](https://github.com/2lucas/api-hacking/assets/82060275/91bd112d-9a38-489e-8353-61cd22525da1)



##### Pentest | Denial of Service
- Um Self-Denial of Service basicamente sobrecarrega a API, permitindo que ela execute operações complicadas que aumentam o consumo e atingem o tempo limite, sobrecarregando o servidor e causando indisponibilidade parcial ou total.
 
 - Podemos explorar a falha enviando uma requisição autenticada para o endpoint `/users/v1/{username}/email` e forneceremos uma longa string que na verdade não é um endereço de e-mail real, na verdade é apenas um payload feito para sobrecarregar o regex.
![Pasted image 20240422195050](https://github.com/2lucas/api-hacking/assets/82060275/06bf1088-4814-41e3-b43b-a7a9a30e6504)

- Como podemos ver, o aplicativo não está mais respondendo (quanto maior a string, mais tempo leva para voltar a funcionar, pode ser necessário reiniciar o serviço).
- Isso é causado pelo fato de o endereço de e-mail ser verificado por meio de uma implementação RegEx que leva muito tempo para verificar se a string inserida é um endereço de e-mail real ou não.
