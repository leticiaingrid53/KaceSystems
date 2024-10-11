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


Exemplo de como chamar a api no fluig com autentização Bearer

			var obtertoken = "";

			$.ajax({
				type: "GET",
				contentType: "application/json",
				url: "/api/public/2.0/authorize/client/test",
				data: { 'serviceCode': 'Kace2' }, /*serviço cadastrado no fluig para login*/
				success: function (a, e, T) {
					obtertoken = a.content.result;
					obtertoken = obtertoken.match(/var csrf_token = "(.*?)"/);

					// Chame o segundo AJAX aqui, após obter o token
					$.ajax({
						type: 'POST',
						crossDomain: true,
						url: 'https://suporte.reframax.com.br/api/service_desk/tickets',
						headers: {
							'Content-Type': 'application/json',
							'x-kace-api-version': '13',
							'Access-Control-Allow-Origin':'*',
							'Access-Control-Allow-Credentials':'true',
							'Access-Control-Allow-Headers':'x-kace-auth-timestamp, x-kace-auth-key, x-kace-auth-signature, accept, origin, content-type',
							'Access-Control-Allow-Methods': 'PUT, DELETE, POST, GET, OPTIONS',
							'Authorization': 'Bearer ' + obtertoken[1]
						},
						data: JSON.stringify({
							"Tickets": [
								{
									"submitter": 0,
									"hd_queue_id": 1,
									"category": 263,
									"title": "My Title - teste api kace",
									"summary": "teste api kace teste"
								}
							]
						}),
						success: function (data) {
							console.log("uhuuuuu");
						},
						error: function (error) {
							console.log("ahhhhh");
							console.log("oiii eu aqui" + error);
						},
					});

				},
				error: function (a, e, T) {
					console.error("ERRO: resposta do ajax: ", a, e, T);
				}
			});

   Teste via console
   
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////  

   // Função assíncrona para realizar as solicitações
   
  async function fetchData() {
  
  // URL para autenticação
  
  const loginUrl = "https://suporte.reframax.com.br/ams/shared/api/security/login";
 
  // Payload com as credenciais de login
  
  const payload = {
    userName: "usuariokace",
    password: "senhakace",
    organizationName: "Default"
  };
 
  // Cabeçalhos da requisição POST
  
  const headers = {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
    'x-dell-api-version': '5'
  };
 
  try {
  
    // Requisição POST para autenticação
    
    const response = await fetch(loginUrl, {
      method: 'POST',
      headers: headers,
      body: JSON.stringify(payload)
    });
 
    // Verificando se a resposta foi bem-sucedida
    if (!response.ok) {
      throw new Error('Erro na autenticação: ' + response.statusText);
    }
 
    // Obtendo o JSON da resposta
    const data = await response.json();
    console.log(JSON.stringify(data, null, 3));
 
    // Obtendo o x-dell-csrf-token dos cabeçalhos da resposta
    const csrfToken = response.headers.get('x-dell-csrf-token');
    console.log("não aguento mais "+csrfToken);
 
    // URL para obter a lista de dispositivos
    const devicesUrl = "https://suporte.reframax.com.br/api/service_desk/tickets";
 
    // Cabeçalhos para a requisição GET
    const getHeaders = {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
      'x-dell-api-version': '5',
      'x-dell-csrf-token': csrfToken
    };
    const raw = JSON.stringify({
      "Tickets": [
        {
          "submitter": 0,
          "hd_queue_id": 1,
          "category": 263,
          "title": "My Title - teste api kace",
          "summary": "teste api kace teste"
        }
      ]
    });
    // Requisição GET para obter a lista de dispositivos
    const devicesResponse = await fetch(devicesUrl, {
      method: 'POST',
      body: raw,
      headers: getHeaders
    });
 
    // Verificando se a resposta foi bem-sucedida
    if (!devicesResponse.ok) {
      throw new Error('Erro ao obter dispositivos: ' + devicesResponse.statusText);
    }
 
    // Obtendo o JSON da resposta
    const devicesData = await devicesResponse.json();
    console.log(JSON.stringify(devicesData, null, 3));
 
  } catch (error) {
    // Tratamento de erro
    console.error('Erro na requisição:', error);
  }
}
 
// Chamando a função para executar as requisições
fetchData();
