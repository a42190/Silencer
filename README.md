<div align="center">
  <p>
    <img width="250" src="https://www.androidpolice.com/wp-content/themes/ap2/ap_resize/ap_resize.php?%20src=https%3A%2F%2Fwww.androidpolice.com%2Fwp-content%2Fuploads%2F2019%2F08%2FAndroid-logo_stacked-RGB.png&w=728">
    <br/>
    <a href="https://github.com/alexandreruivo/Silencer-API-Documentation/releases/download/1/Silencer-beta.apk" target="_blank">           Download Apk
    </a> 
  </p>
  <p>
    <img width="200" src="https://cdn4.iconfinder.com/data/icons/logos-3/600/React.js_logo-512.png">
    <br/>
    <a href="https://silencer-web.a42210isel.now.sh/" target="_blank">
      Visit React App
    </a>
  </p>
</div>


Esta é uma aplicação que visa habilitar qualquer cidadão de criar queixas e acompanhar o estado das mesmas. Para gestão e análise de todas estas queixas existem os operadores de informação. Todas as funcionalidades desta mesma API serão descritas prontamente.
### Participantes
   * [Criar conta](#signup)
   * [Validar conta](#verification)
   * [Entrar na conta](#login)
   * [Aceder à informação do próprio perfil](#personalProfile)
   * [Listar todos os participantes](#allParticipants)
   * [Participante por identificador](#participantById)
   * [Mudar a informação do perfil privado](#changeInfo)
   * [Recuperar o acesso à conta](#recoverPassword)
   * [*Refresh Authentication Token*](#refreshToken)
   * [Atualizar o *Notification Token*](#updatenotificationToken)
   * [Eliminar o *Notification Token*](#deletenotificationToken)
   
### Queixas
   * [Criar queixa](#createComplaint)
   * [Listar todas](#getAllComplaints)
   * [Obter por identificador](#getById)
   * [Atualizar informação da queixa](#updateComplaint)
   * [Listar todas dentro de uma *Bounding Box*](#boxedComplaints)
   * [Listar todas dentro de uma *Bounding Box* com *Clusters*](#boxedwithclustercomplaints)

### Excepções
   * [Not Valid User](#notvaliduserexception)
   * [Authorization](#authorizationexception)
   * [Not Matching Passwords](#notmatchingpasswordsexception)
   * [Body Wrong or Missing](#bodywrongormissingexception)
   * [Not Found](#notfoundexception)
   * [Invalid Parameters](#invalidparametersexception)
   * [Too Many Requests](#toomanyrequestsexception)

# Participantes

## <a name='signup'></a> Criar conta

Criação de conta sem permissão para utilizar o serviço.
        
 * Método **HTTP** a ser utilizado: **_POST_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants**
 * **_Headers_**: Content-Type: application/json
 * **_Authorization_**: *Bearer Token* (Apenas participantes)
 * **_Body_**: 

```json
{
    "name":"João",
    "password":"Umdoistrês4",
    "email":"joaoumdois@sapo.pt",
    "dateOfBirthday":802982689000,
    "cellphoneNumber":910460837
}
```

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "message": "Sign up was successful. Check your inbox with the verification e-mail"
}
```

 * 422 **_Unprocessable Entity_**, quando já existe um utilizador com o mesmo *e-mail* ou número de telefone, ou ainda se o número de telefone não for português, com as respostas em **_Problem+JSON_**, respectivamente:
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notvaliduserexception",
    "title": "Not Valid User",
    "detail": "The given email is taken.",
    "status": 422
}
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The given cellphone number is already taken.",
    "status": 422
}
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The given cellphone number is not portuguese.",
    "status": 422
}
```




## <a name='verification'></a> Validar Conta

Validação da conta através de um token gerado automaticamente pelo servidor   

 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/validation**
 * **_Parameters_** : 
    * email_verification_token - *token* (*String*)
 * **_Authorization_**: *Bearer Token* (Apenas participantes)
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_Json_**
```json
{
    "message": "User successfully authenticated. Login and everything should be fine."
}
```

* 422 **_Unprocessable Entity_**, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "There must be a token to be validated.",
    "status": 422
}
```

 * 422 **_Unprocessable Entity_**, significa que o token fornecido já não é válido.
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The given token has expired. Each token lasts for 24 hours after being generated.",
    "status": 422
}
```



## <a name ='login'></a>Entrar na conta

Autenticação através das credenciais de utilizador.

 * Método **HTTP** a ser utilizado: **_POST_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/login**
 * **_Authorization_**: *Bearer Token* (Ambos)
 * **_Body_**:

```json
{
    "email":"joaoumdois@sapo.pt",
    "password":"Umdoistrês4"
}
```
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**

  Participante
```json
{
    "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzb3BhcmFjc0BnbWFpbC5jb20iLCJhdXRoIjoiUk9MRV9VU0VSIiwiaWF0IjoxNTY3NjI4MTAxLCJleHAiOjE1Njc3MTQ1MDF9.hQm15MDcGlDSNrj4NOCRsH-BRim1jcBIVNKg77zwzz4",
    "role": "USER"
}
```
 Operador de Informação
```json
{
    "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzb3BhcmFjc0BnbWFpbC5jb20iLCJhdXRoIjoiUk9MRV9VU0VSIiwiaWF0IjoxNTY3NjI4MTAxLCJleHAiOjE1Njc3MTQ1MDF9.hQm15MDcGlDSNrj4NOCRsH-BRim1jcBIVNKg77zwzz4",
    "role": "ADMIN"
}
```
 * 400 **_Bad Request_**, quando o valor de email ou password são nulos ou qualquer um destes campos está em falta, com a mensagem de erro em **_Problem+JSON_**
```json
    {
        "type": "https://alexandreruivo.github.io/Silencer/#requestbodymissingorwrongexpetion",
        "title": "Request Body Wrong Or Missing",
        "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
        "status": 400
}
```
* 422 **_Unprocessable Entity_**, quando o valor de email ou password estão vazios , com a mensagem de erro em **_Problem+JSON_**
```json
    {
        "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
        "title": "Invalid Parameters",
        "detail": "Either the email or password are empty.",
        "status": 422
}
```
 * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```
 

## <a name='personalProfile'></a>Aceder à informação do próprio perfil

Apresenta toda a informação relativa ao utilizador, fornecida pelo mesmo.       
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/profile**
 * **_Authorization_**: *Bearer Token* (Apenas participantes)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_Json_**
```json
{
    "id": 1,
    "email": "joaoumdois@sapo.pt",
    "name": "João",
    "dateOfBirthday": 802982689000,
    "cellphoneNumber": 910460837,
    "complaints": [],
    "devices": []
}
``` 
 * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='allParticipants'></a>Listar todos os participantes existentes

Permite ao operador de informação aceder a todos os utilizadores registados no servidor.        
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants**
 * **_Authorization_**: *Bearer Token* (Apenas operadores de informação)
 
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
[
    {
        "id": 2,
        "email": "joaoumdois@sapo.pt",
        "name": "João",
        "dateOfBirthday": 802982689000,
        "cellphoneNumber": 910460837,
        "complaints": [
            {
                "type": "Feature",
                "geometry": {
                    "type": "Point",
                    "coordinates": [
                        38.981609,
                        -9.369435
                    ]
                },
                "properties": {
                    "id": 13,
                    "description": "first complaint",
                    "state": "SUBMITTED",
                    "deviceTime": 1557139305713,
                    "serverTime": 1567557137836,
                    "relevanceLevel": 1.7550293126949535E-9,
                    "decibelValues": [
                        10.0,
                        10.0,
                        10.0
                    ],
                    "complainerId": 2
                }
            },
        ]
    },
    {
        "id": 3,
        "email": "maria@gmail.com",
        "name": "Maria",
        "dateOfBirthday": 841100400000,
        "complaints": [],
        "devices": []
    }
]
```

 * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='participantById'></a>Participante por identificador

Informação de um utilizador em particular.
        
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/\<pid>** - **pid** : Identificador do participante  
 * **_Authorization_**: *Bearer Token* (Apenas operadores de informação)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "id": 2,
    "email": "joaoumdois@sapo.pt",
    "name": "João",
    "dateOfBirthday": 802982689000,
    "cellphoneNumber": 910460837,
    "complaints": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    38.981609,
                    -9.369435
                ]
            },
            "properties": {
                "id": 10,
                "description": "first complaint",
                "state": "SUBMITTED",
                "deviceTime": 1557139305713,
                "serverTime": 1567556833494,
                "relevanceLevel": 1.3651706621769402E-9,
                "decibelValues": [
                    10.0,
                    10.0,
                    10.0
                ],
                "complainerId": 2
            }
        }
    ],
    "devices": []
}
```

 * 404 **_Not Found_**, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notfoundexception",
    "title": "The specific resource you are looking for does not exist",
    "detail": "There is no user with such ID",
    "status": 404
}
```
 * 422 **_Unprocessable Entity_**, quando o perfil a que se tenta aceder pertence a um operador de informação,que não o mesmo que faz o pedido, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The profile tried to access is not reachable.",
    "status": 422
}
 ```
  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='changeInfo'></a>Mudar a informação do perfil privado

 * Método **HTTP** a ser utilizado: **_PUT_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/profile** 
 * **_Authorization_**: *Bearer Token* (Apenas participantes)
 * **_Body_**:

 ```json
{
    "name":"João António",
    "email":"joaoumquatro@gmail.com",
    "currentPassword":"Umdoistrês4",
    "newPassword":"Quatro1cinco",
    "cellphoneNumber":967849258
}
 ```

À excepção do campo 'currentPassword' todos os outros são opcionais, sendo que tem de ser modificado pelo menos um. 

### Resultado retornado para o **_status code_**:

 * 200 **OK**,  com a resposta em **_JSON_**
```json
{
    "id": 2,
    "email": "joaoumquatro@gmail.com",
    "name": "João António",
    "dateOfBirthday": 802982689000,
    "cellphoneNumber": 967849258,
    "complaints": [],
    "devices": []
}
```
 * 422 **_Unprocessable Entity_**, quando o perfil a que se tenta aceder pertence a um operador de informação,que não o mesmo que faz o pedido, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "Make sure you provided your current password to assure authenticity.",
    "status": 422
}
```
 * 401 **_Unauthorized_** quando a *password* providenciada não corresponde com a que está guardada no servidor, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notmatchingpasswordsexception",
    "title": "NotMatchingPasswords",
    "detail": "Your current password does not match with the one provided.",
    "status": 401
}
 ```

 * 422 **_Unprocessable Entity_**, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "Either email, name, password or cellphone number must be not null, while changing information.",
    "status": 422
}
```
* 400 **_Bad Request_**, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#requestbodywrongormissingexception",
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```
  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```


## <a name='recoverPassword'></a>Recuperar acesso à conta

Para recuperar acesso à conta é enviada para o e-mail do participante uma nova password gerada automaticamente pelo servidor. Posteriormente, após realizar o login com a nova password, o participante pode então voltar a mudar se achar necessário.      
 * Método **HTTP** a ser utilizado: **_PUT_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/recoverpassword**
 * **_Body_**:

```json
{
  "email": "joaoumquatro@gmail.com"
}
```
 
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "message": "Check your e-mail inbox for your new password and proceed to login."
}
```

 * 404 **_Not Found_**, quando é enviado um e-mail não existente na base de dados, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notfoundexception",
    "title": "The specific resource you are looking for does not exist",
    "detail": "There is no user with such email",
    "status": 404
}
```

## <a name='refreshToken'></a>*Refresh Authentication Token*

       
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/refreshtoken**
 * **_Authorization_**: *Bearer Token* (Apenas participantes)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "newToken": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzb3BhcmFjc0BnbWFpbC5jb20iLCJhdXRoIjoiUk9MRV9VU0VSIiwiaWF0IjoxNTY3NjM2MjQwLCJleHAiOjE1Njc3MjI2NDB9.3N_T9kHwN0yDjdkgCXKP8TW-JzqP_ADmYICnTIK8pQU"
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='updatenotificationToken'></a> Atualizar o *Notification Token*

Atribuir um novo *token* de forma a receber notificações.    
 * Método **HTTP** a ser utilizado: **_PUT_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/notificationtoken**
 * **_Authorization_**: *Bearer Token* (Ambos)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "message": "All went good. You will start receiving notifications soon enough."
}
```

* 422 **_Unprocessable Entity_**, quando o token enviado está vazio, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The provided token is not valid, it is empty.",
    "status": 422
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```


## <a name='deletenotificationToken'></a> Eliminar o *Notification Token*

Deixar de receber notificações

 * Método **HTTP** a ser utilizado: **_DELETE_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/notificationtoken**
 * **_Authorization_**: *Bearer Token* (Ambos)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
```json
{
    "message": "All went good. You won't be bothered any longer."
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```



# Queixas

## <a name='createComplaint'></a> Criar queixa

 Criar uma nova queixa.

 * Método **HTTP** a ser utilizado: **_POST_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints**
 * **_Authorization_**: *Bearer Token* (Apenas participantes)
 * **_Body_**: 
 ```json
{
	"type":"Feature",
	"geometry": {
        "type": "Point",
        "coordinates": [
            38.981609, -9.369435
        ]
    },
    "properties":{
    	"description" : "first complaint",
		"deviceTime" : "1557139305713",
		"decibelValues" : [10,10,10]
    }
    
}
 ```
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [
            38.981609,
            -9.369435
        ]
    },
    "properties": {
        "id": 15,
        "description": "first complaint",
        "state": "SUBMITTED",
        "deviceTime": 1557139305713,
        "serverTime": 1567689409037,
        "relevanceLevel": 1.0,
        "decibelValues": [
            10.0,
            10.0,
            10.0
        ],
        "complainerId": 3
    }
}
 ```

 * 400 **_Bad Request_**, quando o *payload* não corresponde com o definido em cima, com a mensagem de erro em **_Problem+JSON_**
```json
    {
        "type": "https://alexandreruivo.github.io/Silencer/#requestbodywrongormissingexception",
        "title": "Request Body Wrong Or Missing",
        "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
        "status": 400
}
```

 * 429 **_Too Many Requests_**, quando o utilizador faz mais que 5 queixas nos últimos 15 minutos, coma mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#toomanyrequestsexception",
    "title": "Too Many Requests",
    "detail": "Each user is only allowed a total of 5 complaints every other 15 minutes.",
    "status": 429
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='getAllComplaints'></a> Listar todas as queixas

Listar todas as queixas existentes. Os utilizadores apenas conseguem ver as suas, enquanto os operadores de informação podem ver todas.

 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints**
 * **_Authorization_**: *Bearer Token* (Ambos)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    38.981609,
                    -9.369435
                ]
            },
            "properties": {
                "id": 15,
                "description": "first complaint",
                "state": "SUBMITTED",
                "deviceTime": 1557139305713,
                "serverTime": 1567689409037,
                "relevanceLevel": 1.0,
                "decibelValues": [
                    10.0,
                    10.0,
                    10.0
                ],
                "complainerId": 3
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    38.981609,
                    -9.369435
                ]
            },
            "properties": {
                "id": 7,
                "description": "first complaint",
                "state": "ACCEPTED",
                "deviceTime": 1557139305713,
                "serverTime": 1567556483379,
                "relevanceLevel": 1.3028693023151788E-16,
                "decibelValues": [
                    10.0,
                    10.0,
                    10.0
                ],
                "complainerId": 2
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.060376,
                    -8.17299
                ]
            },
            "properties": {
                "id": 4,
                "description": "first complaint",
                "state": "ACCEPTED",
                "deviceTime": 1557139305713,
                "serverTime": 1567552240921,
                "relevanceLevel": 4.0096081780831594E-17,
                "decibelValues": [
                    10.0,
                    10.0,
                    10.0
                ],
                "complainerId": 2
            }
        },
    ]
}
 ```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='getById'></a> Obter queixa por identificador

Obter queixa por identificador. O utilizador só consegue aceder a queixas feitas pelo mesmo. O operador de informação consegue aceder a qualquer uma.

 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints/\<cid>** - **cid** : Identificador da queixa
 * **_Authorization_**: *Bearer Token* (Ambos)

### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [
            38.978415,
            -8.206972
        ]
    },
    "properties": {
        "id": 1,
        "description": "first complaint",
        "state": "ACCEPTED",
        "deviceTime": 1557139305713,
        "serverTime": 1567526593646,
        "relevanceLevel": 2.733385707086068E-20,
        "decibelValues": [
            10.0,
            10.0,
            10.0
        ],
        "complainerId": 2
    }
}
 ```
 * 404 **_Not Found_**, quando o recurso não é encontrado, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type" : "https://alexandreruivo.github.io/Silencer/#notfoundexception",
    "title": "The specific resource you are looking for does not exist",
    "detail": "There is no complaint with such id for this user.",
    "status": 404
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='updateComplaint'></a> Atualizar informação da queixa

 Atualização de uma queixa.

  * Método **HTTP** a ser utilizado: **_PUT_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints/\<cid>** - **cid** : Identificador da queixa
 * **_Authorization_**: *Bearer Token* (Ambos)
 * **_Body_**:

##### Participante

O Participante apenas tem 10 minutos para mudar a descrição.

 ```json
{
    "description":"something else happened."
}
 ```
##### Operador de Informação
 ```json
{
    "state":"ANALYZING"
}
```

### Resultado retornado para o **_status code_**:

* 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [
            38.978415,
            -8.206972
        ]
    },
    "properties": {
        "id": 1,
        "description": "something else happened.",
        "state": "ANALYZING",
        "deviceTime": 1557139305713,
        "serverTime": 1567526593646,
        "relevanceLevel": 2.733385707086068E-20,
        "decibelValues": [
            10.0,
            10.0,
            10.0
        ],
        "complainerId": 2
    }
}
 ```

* 400 **_Bad Request_**, quando os campos do body não são 'description', no caso do utilizador, e 'state', no caso do operador de informação, com a mensagem de erro em **_Problem+JSON_**
```json
    {
        "type": "https://alexandreruivo.github.io/Silencer/#requestbodywrongormissingexception",
        "title": "Request Body Wrong Or Missing",
        "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
        "status": 400
}
```

  * 404 **_Not Found_**, quando não existe a queixa com o identificador providenciado,com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notfoundexception",
    "title": "The specific resource you are looking for does not exist",
    "detail": "The complaint with that id does not exist.",
    "status": 404
}
 ```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

* 422 **_Unprocessable Entity_**, quando o identificador da queixa não pertence ao utilizador autenticado,com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The given complaint do not belong to the authenticated user",
    "status": 422
}
 ```
* 422 **_Unprocessable Entity_**, quando já passaram 10 minutos após ser efectuada a queixa, com a mensagem de erro em **_Problem+JSON_**
 ```json
 {
    "title": "Invalid Parameters",
    "detail": "Sorry, 10 minutes has passed. You can no longer change the description.",
    "status": 422
}
```

## <a name='boxedComplaints'></a> Listar todas dentro de uma *Bounding Box*

 Listar todas as queixas dentro de uma *box* feita à custa de coordenadas fornecidas pelo operador de informação.

  * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints**
 * **_Parameters_**: 
   * northEastLat - Coordenada (*Double*) 
   * northEastLng - Coordenada (*Double*) 
   * southWestLat - Coordenada (*Double*)  
   * southWestLng - Coordenada (*Double*) 
 * **_Authorization_**: *Bearer Token* (Apenas operador de informação)
 
### Resultado retornado para o **_status code_**:

* 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    38.978415,
                    -8.206972
                ]
            },
            "properties": {
                "complaintId": 1,
                "relevanceLevel": 1.6826499727874375E-20,
                "serverTime": 1567526593646,
                "decibelAverage": 10.0
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.157259,
                    -8.176262
                ]
            },
            "properties": {
                "complaintId": 3,
                "relevanceLevel": 2.0783858959518397E-17,
                "serverTime": 1567552221963,
                "decibelAverage": 10.0
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.060373,
                    -8.172984
                ]
            },
            "properties": {
                "complaintId": 5,
                "relevanceLevel": 2.0992250389824976E-17,
                "serverTime": 1567552257879,
                "decibelAverage": 10.0
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.060376,
                    -8.17299
                ]
            },
            "properties": {
                "complaintId": 4,
                "relevanceLevel": 2.0893597764882437E-17,
                "serverTime": 1567552240921,
                "decibelAverage": 10.0
            }
        },
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.178658,
                    -8.187146
                ]
            },
            "properties": {
                "complaintId": 2,
                "relevanceLevel": 2.06836402721244E-17,
                "serverTime": 1567552204561,
                "decibelAverage": 10.0
            }
        }
    ]
}
 ```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='boxedwithclustercomplaints'></a> Listar todas dentro de uma *Bounding Box* com *Clusters*

 Listar todas as queixas dentro de uma *box* feita à custa de coordenadas fornecidas pelo operador de informação, sendo posteriormente agrupadas e é formado um *cluster* baseado na distância fornecida pelo operador.

  * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/complaints**
 * **_Parameters_**: 
   * northEastLat - Coordenada (*Double*) 
   * northEastLng - Coordenada (*Double*) 
   * southWestLat - Coordenada (*Double*)  
   * southWestLng - Coordenada (*Double*) 
   * distance - Metros (*Double*)
   * minPoints - Quantidade (*Int*)
   * beginDate - Data (*Long*)(Opcional)
   * endDate - Data (*Long*)(opcional)
   * averageDecibel - decibel (*Double)(Opcional)
 * **_Authorization_**: *Bearer Token* (Apenas operador de informação)

### Resultado retornado para o **_status code_**:

* 200 **OK**, com a resposta em **_JSON_**
 ```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "properties": {
                "clusterId": 0,
                "numGeom": 1
            },
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.060373,
                    -8.172984
                ]
            }
        },
        {
            "type": "Feature",
            "properties": {
                "clusterId": 1,
                "numGeom": 1
            },
            "geometry": {
                "type": "Point",
                "coordinates": [
                    39.060376,
                    -8.17299
                ]
            }
        }
    ]
}
 ```


* 422 **_Unprocessable Entity_**, quando as datas enviadas não são válidas, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "The given dates are not valid.",
    "status": 422
}
```
  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

# Excepções

## <a name='notvaliduserexception'></a> *Not Valid User Exception*

 Quando o email ao qual se está a tentar associar já existe na base de dados do servidor.

 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notvaliduserexception",
    "title": "Not Valid User",
    "detail": "The given email is taken.",
    "status": 422
}
 ```

## <a name='authorizationexception'></a> *Authorization Exception*

 Quando as credenciais apresentadas, sejam elas *e-mail* e *password* ou *token*, estão erradas.
 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#authorizationexception",
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted.",
    "status": 401
}
 ```

## <a name='notmatchingpasswordsexception'></a> *Not Matching Passwords Exception*

Quando a password apresentada para alterar as informações do utilizador não corresponde à password guardada na base de dados.

 ```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notmatchingpasswordsexception",
    "title": "NotMatchingPasswords",
    "detail": "Your current password does not match with the one provided.",
    "status": 401
}
 ```

## <a name='bodywrongormissingexception'></a> *Body Wrong Or Missing Exception*

Quando o conteúdo do *body* não corresponde ao pretendido.

```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#bodywrongormissingexception",
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```

## <a name='notfoundexception'></a> *Not Found Exception*

Quando recurso desejado não existe registado.
```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#notfoundexception",
    "title": "The specific resource you are looking for does not exist",
    "detail": "detailed message could vary.",
    "status": 404
}
```

## <a name='invalidparametersexception'></a> *Invalid Parameters Exception*

Quando os parâmetros fornecidos são inválidos.

```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#invalidparametersexception",
    "title": "Invalid Parameters",
    "detail": "detailed message could vary.",
    "status": 422
}
```

## <a name='toomanyrequestsexception'></a> *Too Many Requests Exception*

Quando os parâmetros fornecidos são inválidos.

```json
{
    "type": "https://alexandreruivo.github.io/Silencer/#toomanyrequestsexception",
    "title": "Too Many Requests",
    "detail": "Each user is only allowed a total of 5 complaints every other 15 minutes.",
    "status": 429
}
```
