---
>
---

---

# DOCUMENTAÇÃO

## A API contém 3 Endpoints: uma de usuários (/users), uma de carros (/cars) e uma de imóveis (/properties).

## USUÁRIOS - USERS

Somente é possível ter acesso aos usuários se estiver cadastrado.

Assim, inicialmente, é indicado fazer um registro.

### REGISTRO

`POST /user/register `

#### Formato da requisição:

```json
{
  "email": "email@valido.com",
  "password": "123456",
  "firstname": "Nome",
  "lastname": "Sobrenome"
}
```

Os itens "firstname" e "lastname" não são obrigatórios.

#### Resposta esperada:

`POST /user/register - FORMATO DA RESPOSTA - STATUS 201`

```json
{
"accessToken": "{token de acesso JWT}",
"user": {
"email": "email@valido.com",
"firstname": "Nome",
"lastname": "Sobrenome",
"id": {número conforme cadastro}
}
}
```

#### Possíveis erros:

> "Email format is invalid"
>
> O email deve ter o formato padrão, com @ e .com ao final. Exemplo: teste@teste.com

> "Email and password are required"
>
> Tanto email quando senha são obrigatórios para o registro.

> "Password is too short"
>
> Senha deve ter ao menos 4 caracteres.

> "Email already exists"
>
> Email já está sendo utilizado por algum usuário. Tentar utilizar outro.

### LOGIN

`POST /login`

#### Formato da requisição:

```json
{
  "email": "email@valido.com",
  "password": "123456"
}
```

#### Resposta esperada:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
"accessToken": "token de acesso no formato JWT",
"user": {
"email": "email@valido.com",
"firstname": "Nome",
"lastname": "Sobrenome",
"id": {número conforme cadastro}
}
}
```

#### Possíveis erros:

> "Incorrect password"
>
> A senha está diferente da cadastrada. Verificar se digitou corretamente.

> "Cannot find user"
>
> O email informado não está cadastrado no sistema. Verifique se o email está digitado corretamente.

---

## CARROS - CARS

### CADASTRO

Para a cadastro de veículos é necessário que o usuário esteja logado. Assim, é necessário informar o acessToken como Bearer.

`POST /carros`

#### Formato da requisição:

```json
{
  "model": "Modelo 5",
  "year": 2022,
  "userId": 2
}
```

Explicação sobre os dados:

- model: string que contém os dados do veículo como marca e modelo
- year: número com 4 digitos referente ao ano de fabricação do veiculo
- userId: número referente ao if do usuário, conforme cadastro realizado.

Apenas o userId é **OBRIGATÓRIO**.

`POST /cars - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "model": "{dados do veículo como marca e modelo}",
  "year": {ano no formato (yyyy) de fabricação do veículo},
  "userId": {id do usuário, conforme cadastro realizado},
  "id": {id o veículo cadastrado}
}
```

#### Possíveis erros:

> "Private resource creation: request body must have a reference to the owner id"
>
> Não foi informado o userId para cadastro do carro.

### PESQUISA

Para realizar a pesquisa de todos os veículos cadastrados, não é necessário estar logado.

`GET /cars - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "model": "Modelo",
    "year": 2022,
    "userId": 2,
    "id": 1
  },
  {
    "model": "Modelo 2",
    "year": 2022,
    "userId": 2,
    "id": 2
  },
  {
    "model": "Modelo 3",
    "year": 2022,
    "userId": 2,
    "id": 3
  },
  {
    "model": "Modelo 4",
    "year": 2022,
    "userId": 2,
    "id": 4
  },
  {
    "model": "Modelo 5",
    "year": 2022,
    "userId": 2,
    "id": 5
  }
]
```

#### FILTRO COM QUERY PARAMS

É possivel filtrar os veículos através do uso de query params e, para tal, somente usar, depois do endpoint, o tipo de fitro que quer usar (model, year, id ou userId)

Exs.:

- Para o filtro de veículos de certo modelo:
  /cars?model=Modelo
- Para o filtro de veículos de certo ano:
  /cars?year=2022
- Para o filtro de veículos de certo usuario:
  /cars?userId=1
- Para o filtro de veículos através do Id cadastrado:
  /cars?id=1

---

## IMÓVEIS - PROPERTIES

### CADASTRO

Assim como no cadastro de novos veículos, para realizar o cadastro de imóveis também é necessário que o usuário esteja logado e, consequentemente, deve informar o acessToken como Bearer.

`POST /properties`

#### Formato da requisição:

```json
{
  "type": "apartament",
  "area": 70,
  "value": 150000,
  "userId": 3
}
```

Explicação sobre os dados:

- type: string contendo se o imóvel é apartamento ou casa
- area: numero informando a area em metros quadrados
- value: o valor avaliado do imóvel
- userId: id do usuário, conforme cadastro realizado

Apenas o userId é **OBRIGATÓRIO**.

`POST /properties - FORMATO DA RESPOSTA - STATUS 201`

```json
{
	"type" : "{apartment or house}",
	"area": {numero informando a area em metros quadrados},
	"value": {o valor avaliado do imóvel},
    "userId": {id do usuário, conforme cadastro realizado},
    "id": {id do imóvel cadastrado}
}
```

#### Possíveis erros:

> "Private resource creation: request body must have a reference to the owner id"
>
> Não foi informado o userId para cadastro do carro.

### PESQUISA

Para realizar a pesquisa de todos os veículos cadastrados, não é necessário estar logado.

`GET /properties - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "userId": 3,
    "type": "apartment or house",
    "area": 0,
    "value": 10,
    "id": 1
  },
  {
    "type": "apartment or house",
    "area": 0,
    "value": 10,
    "userId": 3,
    "id": 2
  }
]
```

#### FILTRO COM QUERY PARAMS

É possivel filtrar os imóveis através do uso de query params e, para tal, somente usar, depois do endpoint, o tipo de fitro que quer usar (type, area, id ou userId)

Exs.:

- Para o filtro de veículos de certo type:
  /cars?type=house
- Para o filtro de veículos de certo área:
  /cars?area=220
- Para o filtro de veículos de certo usuario:
  /cars?userId=1
- Para o filtro de veículos através do Id cadastrado:
  /cars?id=1

---

---

---
