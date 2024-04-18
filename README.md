# KaceSystems
Como realizar criação de ticket via api Kace Systems

Primeiro realize o login utilizando a api 
Type: POST
URL: {host}/ams/shared/api/security/login
Exemplo:
https://suporte.empresa.com.br/ams/shared/api/security/login

Syntax:
• Type: POST
• URL: {host}/ams/shared/api/security/login
• Headers:
◦ Content-Type:application/json
◦ Accept:application/json
• Body:
◦ {"userName": "{username}", "password": "{password}"}

Para chamada da api via postman é necessário adicionar o Content-Length para que funcione a chamada 
![image](https://github.com/leticiaingrid53/KaceSystems/assets/56976378/a11a11a3-5808-4a52-8579-3a59ceccf763)
No body do kace deve ser selecionada a opção raw ===> JSON
Para login é necessário que o usuário seja administrador

Para realizar a criação de um ticket é utilizada a api /api/service_desk/tickets

Syntax:
• Type: POST
• URL: {host}/api/service_desk/tickets
• Headers:
◦ Content-Type:application/json
◦ Accept:application/json
• Body:
◦ { "Tickets":  [
{
"submitter":  0, /*campo obrigatório*/
"hd_queue_id":  1, /*id da fila ex:sede, obras*/
"category":  263, /*id da categoria do chamado*/
"title":  "My Title - teste api kace", /*titulo*/
"summary":"teste api kace teste" /*descrição do chamado*/
}
]
}



