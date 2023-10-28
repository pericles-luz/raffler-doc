# raffler-doc
API para sorteio de amigo oculto

## Fluxo definido para o sorteio

### 1. Criar um sorteio

O administrador cria um sorteio, dando o nome a ser usado. Após registrado o sorteio, a API retorna um link para o sorteio, que será usado para o cadastro dos participantes. Também retorna um link para acesso do administrador, onde ele poderá ver os participantes cadastrados e sortear.

### 2. Cadastrar participantes

Cada participante acessa o link do sorteio e se cadastra, informando seu nome. A API retorna um link para o participante, onde ele poderá ver o amigo oculto que lhe foi sorteado. Não serão permitidos dois participantes com o mesmo nome.

### 3. Sortear

O administrador acessa o link do sorteio onde pode gerenciar os participantes e sortear. O sorteio é feito de forma aleatória, garantindo que cada participante tenha um amigo oculto e que ninguém tire a si mesmo.

Após o sorteio, o administrador pode ver quais participantes já viram seus amigos ocultos e quais ainda não viram. 

### 4. Ver amigo oculto

O participante acessa o link do sorteio e informa seu nome. A API retorna o nome do amigo oculto que lhe foi sorteado. O participante pode acessar o link quantas vezes quiser, até que ele mesmo marque para não ver mais.

## API

### Criar um sorteio

```
POST /raffles
```

#### Payload

```json
{
  "name": "Nome do sorteio"
}
```

#### Resposta

```json
{
  "message": "sorteio registrado com sucesso. Agora encaminhe o link abaixo para os participantes se cadastrarem",
  "data": {
      "id": "139a8846c-2a6f-4602-9176-79ee4e5e3059",
      "register_link": "http://localhost:3000/raffles/139a8846c-2a6f-4602-9176-79ee4e5e3059/register",
      "admin_link": "http://localhost:3000/raffles/139a8846c-2a6f-4602-9176-79ee4e5e3059/admin"
  }
}
```

### Cadastrar participante

```
POST /raffles/:id/register
```

#### Payload

```json
{
  "name": "Nome do participante"
}
```

#### Resposta

```json
{
    "message": "participante cadastrado com sucesso. Nolink abaixo você pode ver o seu amigo oculto depois que o sorteio for realizado",
    "data": {
        "id": "5a6a0b4c-2a6f-4602-9176-79ee4e5e3059",
        "result_link": "http://localhost:3000/raffles/5a6a0b4c-2a6f-4602-9176-79ee4e5e3059/see"
    }
}
```

### Ver amigo oculto

```
GET /raffles/participants/:id/see
```

#### Resposta

```json
{
    "message": "seu amigo oculto é: Fulano"
}
```

### Sortear

```
POST /raffles/:id/draw
```

#### Resposta

```json
{
    "message": "sorteio realizado com sucesso. Agora você pode ver os participantes que já viram seus amigos ocultos e os que ainda não viram",
}
```

### Ver participantes que já viram seus amigos ocultos

```
GET /raffles/:id/seen
```

#### Resposta

```json
{
    "message": "participantes que já viram seus amigos ocultos",
    "data": [
        {
            "id": "5a6a0b4c-2a6f-4602-9176-79ee4e5e3059",
            "name": "Fulano",
            "participate": true,
            "seen": true
        },
        {
            "id": "5a6a0b4c-2a6f-4602-9176-79ee4e5e3059",
            "name": "Ciclano",
            "participate": true,
            "seen": false
        }
    ]
}
```

### Definir se o participante será sorteado ou não

```
PUT /raffles/participants/:id/participate
```

#### Payload

```json
{
  "participate": true
}
```

#### Resposta

```json
{
    "message": "participante atualizado com sucesso",
    "data": {
        "id": "5a6a0b4c-2a6f-4602-9176-79ee4e5e3059",
        "name": "Fulano",
        "participate": true
    }
}
```