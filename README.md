<div align="center">
  <p>
    <img width="250" src="https://www.androidpolice.com/wp-content/themes/ap2/ap_resize/ap_resize.php?%20src=https%3A%2F%2Fwww.androidpolice.com%2Fwp-content%2Fuploads%2F2019%2F08%2FAndroid-logo_stacked-RGB.png&w=728">
    <br/>
    <a href="https://github.com/alexandreruivo/Silencer-API-Documentation/releases/download/1/Silencer-beta.apk">           Download Beta Apk
    </a> 
  </p>
  <p>
    <img width="200" src="https://cdn4.iconfinder.com/data/icons/logos-3/600/React.js_logo-512.png">
    <br/>
    <a href="https://silencer-web.a42210isel.now.sh/">
      Visit React App
    </a>
  </p>
</div>

Esta é uma aplicação que visa habilitar qualquer cidadão de criar queixas e acompanhar o estado das mesmas. Para gestão e análise de todas estas queixas existem os operadores de informação. Todas as funcionalidades desta mesma API serão descritas prontamente.

### Participants
   * [Criar conta](#participantSignUp)
   * [Validar conta](#verification)
   * [Entrar na conta](#login)
   * [Aceder à informação do próprio perfil](#personalProfile)
   * [Listar todos os participantes](#allParticipants)
   * [Participante por identificador](#participantById)
   * [Mudar a informação do perfil privado](#changeInfo)
   * [Recuperar o acesso à conta](#recoverPassword)
   * [*Refresh Token*](#refreshToken)
   * [Atualizar o *Notification Token*](#updatenotificationToken)
   * [Eliminar o *Notification Token*](#deletenotificationToken)

## <a name='participantSignUp'></a> Criar conta

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

 * 422 **_Unprocessable Entity_** - Quando já existe um utilizador com o mesmo *e-mail* ou número de telefone, ou ainda se o número de telefone não for português, com as respostas em **_Problem+JSON_**, respectivamente:
```json
{
    "title": "Invalid User",
    "detail": "The given email is taken.",
    "status": 422
}
{
    "title": "Invalid Parameters",
    "detail": "The given cellphone number is already taken.",
    "status": 422
}
{
    "title": "Invalid Parameters",
    "detail": "The given cellphone number is not portuguese.",
    "status": 422
}
```




## <a name='verification'></a>Validar Conta

Validação da conta através de um token gerado automaticamente pelo servidor   

 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/validation**
 * **Parameters** : email_verification_token
 * **_Authorization_**: *Bearer Token* (Apenas participantes)
### Resultado retornado para o **_status code_**:

 * 200 **OK**, com a resposta em **_Json_**
```json
{
    "message": "User successfully authenticated. Login and everything should be fine."
}
```

* 400 **_Bad Request_**, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Empty Field",
    "detail": "There must be a token to be validated.",
    "status": 400
}
```

 * 422 **_Unprocessable Entity_**- significa que o grupo especificado não foi encontrado.
```json
{
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
 * 400 **_Bad Request_** quando o valor de email ou password são nulos ou qualquer um destes campos está em falta, com a mensagem de erro em **_Problem+JSON_**
```json
    {
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```
* 400 **_Bad Request_** quando o valor de email ou password estão vazios , com a mensagem de erro em **_Problem+JSON_**
```json
    {
    "title": "Invalid Request",
    "detail": "Either the email or password are empty.",
    "status": 400
}
```
 * 401 **_Unauthorized_** quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```
 

## <a name='personalProfile'></a>Aceder à informação do próprio perfil

Apresenta toda a informação relativa ao interveniente, fornecida pelo mesmo.       
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
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
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
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```

## <a name='participantById'></a>Participante por identificador

Informação de um utilizador em particular.
        
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _localhost_
 * **Port**: 3000
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
    "title": "The specific resource you are looking for does not exist",
    "detail": "There is no user with such ID",
    "status": 404
}
```
 * 422 **_Unprocessable Entity_**, quando o perfil a que se tenta aceder pertence a um operador de informação,que não o mesmo que faz o pedido, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Invalid Parameters",
    "detail": "The profile tried to access is not reachable.",
    "status": 422
}
 ```
  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```

## <a name='changeInfo'></a>Mudar a informação do perfil privado
Mostra todos os detalhes de um determinado grupo.        
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
    "title": "Invalid Parameters",
    "detail": "Make sure you provided your current password to assure authenticity.",
    "status": 422
}
```
 * 401 **_Unauthorized_** quando a *password* providenciada não corresponde com a que está guardada no servidor, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "NotMatchingPasswords",
    "detail": "Your current password does not match with the one provided.",
    "status": 401
}
 ```


 * 400 **_Bad Request_**, quando a *body* está vazio, nenhum dos campos corresponde aos referidos anteriormente ou o campo password não é enviado, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "title": "Missing Required Information",
    "detail": "Make sure you provided your current password to assure authenticity.",
    "status": 400
}
```

 * 400 **_Bad Request_**, com a mensagem de erro em **_Problem+JSON_**
```json
{
    "title": "Invalid Request",
    "detail": "Either email, name, password or cellphone number must be not null, while changing information.",
    "status": 400
}
```
  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
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
    "title": "The specific resource you are looking for does not exist",
    "detail": "There is no user with such email",
    "status": 404
}
```

## <a name='refreshToken'></a>*Refresh Token*

Adiciona uma equipa existente a um grupo.        
 * Método **HTTP** a ser utilizado: **_GET_**
 * **_Hostname_**: _silencer.ga_
 * **Port**: 9443
 * **_URI_**: **silencerapi/participants/refreshtoken**
 * **_Authorization_**: *Bearer Token* (Apenas participantes)

### Resultado retornado para o **_status code_**:

 * 200 **OK** com a resposta em **_JSON_**
```json
{
    "newToken": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzb3BhcmFjc0BnbWFpbC5jb20iLCJhdXRoIjoiUk9MRV9VU0VSIiwiaWF0IjoxNTY3NjM2MjQwLCJleHAiOjE1Njc3MjI2NDB9.3N_T9kHwN0yDjdkgCXKP8TW-JzqP_ADmYICnTIK8pQU"
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
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
    "title": "Invalid Parameters",
    "detail": "The provided token is not valid, it is empty.",
    "status": 422
}
```

  * 401 **_Unauthorized_**, quando as credenciais inseridas não são válidas, com a mensagem de erro em **_Problem+JSON_**
 ```json
{
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```

## <a name='deletenotificationToken'></a> Eliminar o *Notification Token*

Atribuir um novo *token* de forma a receber notificações.    
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
    "title": "Authorization required",
    "detail": "Access was denied because the required authorization was not granted",
    "status": 401
}
 ```
