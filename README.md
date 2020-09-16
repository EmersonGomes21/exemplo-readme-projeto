# ProjetoX Descritivo
> O ProjetoX consiste em .....
> tornando a experiência de compra mais eficaz e em uma única solução tecnológica.


## Pré-requisitos

* Java 11
* Maven 3.6+
* SQL Server 2019 (15.x)

### Paramêtros Heap a serem passados ao rodar aplicação
* Xms = 256m
* Xmx = 768m

### Endpoints 
* URL Base
    * URL Base Servidor: https://url-de-producao.com
    * URL Base Local: http://localhost:8080
* URL Endpoints
    * /auth/login
    * /signup/register
    * /signup/register-pjinfo
    * /signup/registerupload


### Configurações
```json
SPRING_APPLICATION_JSON='{
	"projetox":{
		"datasource":{
			//URL do Servidor SQL
			"url": "",
			//Usuário do SQL
			"username": "",
			//Senha do SQL
			"password": "",
			//Nome da Instância do SQL
			"instanceName": "",
			//Nome do Banco no Servidor SQL
			"name": "",
			//Porta de Conexão ao Servidor SQL
			"port": ""
		},
		"azure":{
			//String de Conexão Azure SDK
			"connectionString": "",
			//Nome da Conta no Azure
			"accountName":"",
			//URL Base do Azure
			"baseUrl":"",
			//Nome do Container de Uploads Privados
			"privateContainer":"",
			//Nome do Container de Uploads Públicos
			"publicContainer":""
		}
	},
	"jwt":{
		//Segredo JWT para Gerar e Validar Tokens
		"secret":"",
		//Tempo de Expiração dos Tokens em Minutos
		"expiration":""
	},
	//Habilitar/Desabilitar execução das Migrações de Banco de Dados
	"spring.flyway.enabled": false
}'
```

## Rodar o Projeto Em Desenvolvimento

## Levantar Banco de Dados SqlServer 2019 com Docker
```shell
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MsSql!Server01" \
   -p 1433:1433 --name sql1 \
   -d mcr.microsoft.com/mssql/server:2019-CU1-ubuntu-16.04 \
   --restart=unless-stopped
```

Passar a Senha e a porta escolhida ao levantar o container docker na Variavel de configuração:
```json
datasource:{
	"password":"MsSql!Server01", "port": 1433
}
```

### Rodar a API em modo desenvolvimento 
Obs.: Ao rodar pela primeira vez caso queira que as migrações sejam rodadas mude a variável de configuração:  
```"spring.flyway.enabled"``` para "true"

#### Definir variável de Ambiente com o conteúdo do da Sessão #Configurações desse Readme e executar o seguinte comando:

```mvn -q spring-boot:run```

##### Rodar a API em modo Desenvolvimento diretamente sem definir variável de ambiente com o comando abaixo: 

```SPRING_APPLICATION_JSON='{"projetox":{"datasource":{"url":"","username":"","password":"","instanceName":"","name":"","port": ""},"azure":{"connectionString":"","accountName":"","baseUrl":"","privateContainer":"","publicContainer":""}},"jwt":{"secret":"","expiration":""}}' mvn -q spring-boot:run```


## Build do Projeto - Efetuar o Build e Executar
### Efetuar o Build

```mvn clean package -DskipTests spring-boot:repackage```

##### Executar o projeto em modo Produção
###### Modo 1 - Definir Variavel de ambiente e rodar o comando a baixo:

```java -jar target/webapi-0.0.1.jar```

###### Modo 2 - Rodar o comando a baixo que já contém as configurações:

```SPRING_APPLICATION_JSON='{"projetox":{"datasource":{"url":"","username":"","password":"","instanceName":"","name":"","port": ""},"azure":{"connectionString":"","accountName":"","baseUrl":"","privateContainer":"","publicContainer":""}},"jwt":{"secret":"","expiration":""}}' java -jar target/webapi-0.0.1.jar```


## Testar os Endpoints após Levantar a Aplicação
Importar no Postman os seguintes arquivos que estão na raiz do projeto:
* Coleção: ProjetoX-Dev.postman_collection.json
* Environment: ProjetoX-Dev-Env.postman_environment.json

- signup/register -> Registro da Pessoa Física
- signup/register-pjinfo -> Registro da Pessoa Jurídica
- signup/registerupload -> Registro dos Documentos da Pessoa Jurídica 
- Auth login -> Autenticação da Pessoa Física Registrada
- usuario/me -> Endpoint de verificação do status do registro da Pessoa Jurídica

> Os endpoints "signup/register" e "Auth login" são os únicos,  
> que não necessitam de token para serem executados,  
> os demais precisam do cabeçalho "Authorization: Bearer ...." para serem executados,  
> Usando a Coleção e o Environment acima mencionados,  
> após executar "signup/register" e/ou "Auth login"  
> o token retornado é automaticamente atualizado como variável de ambiente do PostMan para o "Environment" importado
