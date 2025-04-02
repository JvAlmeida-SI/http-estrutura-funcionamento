# Protocolo HTTP: Estrutura e Funcionamento

## Estrutura do Protocolo HTTP

O protocolo HTTP (Hypertext Transfer Protocol) é um protocolo de camada de aplicação para transmissão de documentos hipermídia (como HTML). Ele é a base de todas as trocas de dados na Web, e é um protocolo cliente-servidor (as requisições são iniciadas no destinatário final).

Clientes e servidores se comunicam por mensagens individuais. Sua estrutura, de forma mais simplificada, consiste em:

1. **Request / Requisição**: Enviada pelo cliente (navegador, app, etc.)
2. **Response / Resposta**: Enviada pelo servidor em resposta à requisição

### Estrutura de uma Request HTTP:
```
<Método> <URL> <Versão do HTTP>
<Headers>

<Payload> (opcional)
```
**Exemplo:** GET /api/users?id=123 HTTP/1.1

### Estrutura de uma Response HTTP:
```
<Versão do HTTP> <Código de Status> <Mensagem de Status>
<Headers>

<Payload> (opcional)
```
**Exemplo:** HTTP/1.1 200 OK

## Diferença entre HTTP e HTTPS

- **HTTP**: 
  - Comunicação em texto plano (não criptografada)
  - Porta padrão 80
  - Vulnerável a ataques MITM (Man-in-the-Middle)
    - Um invasor intercepta e manipula a comunicação entre duas partes (como cliente e servidor) sem que elas percebam.

- **HTTPS**: 
  - HTTP sobre SSL/TLS (comunicação criptografada)
  - Porta padrão 443
  - Autentica a identidade do servidor
  - Protege a integridade e confidencialidade dos dados
  - Maior segurança para o usuário

## Headers

Headers são metadados que fornecem informações adicionais sobre a requisição ou resposta. Eles são formados por pares nome-valor e são divididos em:

- **Request Headers**: Contêm informações sobre o recurso a ser obtido e o cliente
  - Exemplos: `User-Agent`, `Accept`, `Authorization`

- **Response Headers**: Contêm informações sobre a resposta
  - Exemplos: `Content-Type`, `Set-Cookie`, `Cache-Control`

- **General Headers**: Aplicáveis tanto a requisições quanto a respostas
  - Exemplos: `Date`, `Connection`

- **Entity Headers**: Descrevem o conteúdo do corpo da mensagem
  - Exemplos: `Content-Length`, `Content-Type`

## Payload

Também chamado de "corpo da mensagem", contém os dados transmitidos:

- Em **requests**: Pode conter dados enviados ao servidor (como em POST ou PUT)
- Em **responses**: Contém o recurso solicitado ou dados de retorno

O payload é separado dos headers por uma linha em branco e seu formato é indicado pelo header `Content-Type`.

## Response

Uma resposta HTTP contém:

1. **Status Line**: Versão HTTP, código de status e mensagem
   - Ex: `HTTP/1.1 200 OK`

2. **Headers**: Metadados sobre a resposta

3. **Body**: Conteúdo da resposta (opcional)

### Os códigos de status são divididos em categorias:
**1xx - Respostas Informativas**

Indicam que a requisição foi recebida e o processo continua.

| Código | Nome                     | Descrição                                                                 |
|--------|--------------------------|---------------------------------------------------------------------------|
| 100    | Continue                 | O servidor recebeu os headers e o cliente pode prosseguir com o request   |
| 101    | Switching Protocols      | O servidor concorda em mudar de protocolo conforme solicitado            |
| 102    | Processing               | O servidor está processando a requisição mas ainda não há resposta        |
| 103    | Early Hints              | Indica ao cliente que alguns headers estão disponíveis antecipadamente    |
 
 -----
**2xx - Respostas de Sucesso**

Indicam que a requisição foi recebida, entendida e processada com sucesso.

| Código | Nome                     | Descrição                                                                 |
|--------|--------------------------|---------------------------------------------------------------------------|
| 200    | OK                       | Requisição bem-sucedida (o significado depende do método HTTP usado)     |
| 201    | Created                  | Recurso foi criado com sucesso (comum em respostas a POST)                |
| 202    | Accepted                 | Requisição foi aceita para processamento mas ainda não concluída         |
| 204    | No Content               | Sucesso mas não há conteúdo para retornar                                 |
| 206    | Partial Content          | O servidor está retornando apenas parte do recurso solicitado            |

-----
**3xx - Redirecionamentos**

Indicam que ações adicionais precisam ser tomadas para completar a requisição.

| Código | Nome                     | Descrição                                                                 |
|--------|--------------------------|---------------------------------------------------------------------------|
| 301    | Moved Permanently        | Recurso foi movido permanentemente para nova URL                         |
| 302    | Found                    | Recurso foi movido temporariamente para nova URL                         |
| 304    | Not Modified             | Indica que o recurso não foi modificado desde a última requisição        |
| 307    | Temporary Redirect       | Similar ao 302 mas preserva o método HTTP original                       |
| 308    | Permanent Redirect       | Similar ao 301 mas preserva o método HTTP original                       |

-----
**4xx - Erros do Cliente**

Indicam que houve erro aparentemente causado pelo cliente.

| Código | Nome                     | Descrição                                                                 |
|--------|--------------------------|---------------------------------------------------------------------------|
| 400    | Bad Request              | Requisição mal formada ou inválida                                       |
| 401    | Unauthorized             | Autenticação falhou ou não foi fornecida                                 |
| 403    | Forbidden                | Cliente não tem permissão para acessar o recurso                         |
| 404    | Not Found                | Recurso solicitado não foi encontrado                                    |
| 405    | Method Not Allowed       | Método HTTP não é suportado para este recurso                            |
| 429    | Too Many Requests        | Cliente excedeu o limite de requisições                                  |

-----
**5xx - Erros do Servidor**

Indicam que o servidor falhou ao cumprir uma requisição aparentemente válida.

| Código | Nome                     | Descrição                                                                 |
|--------|--------------------------|---------------------------------------------------------------------------|
| 500    | Internal Server Error    | Erro genérico quando o servidor encontra uma condição inesperada         |
| 501    | Not Implemented          | O servidor não suporta a funcionalidade solicitada                       |
| 502    | Bad Gateway              | Servidor atuando como gateway recebeu resposta inválida                  |
| 503    | Service Unavailable      | Servidor não está pronto para manipular a requisição (sobrecarga/manutenção) |
| 504    | Gateway Timeout          | Servidor atuando como gateway não recebeu resposta a tempo               |



## Parâmetros na Request URL

Parâmetros podem ser incluídos na URL de duas formas:

1. **Query Parameters**: Após o `?` na URL
   - Formato: `?chave=valor&chave2=valor2`
   - Ex: `https://api.com/users?page=2&limit=10`

2. **Path Parameters**: Parte integrante do caminho da URL
   - Ex: `https://api.com/users/123` (onde 123 é o ID do usuário)

## Métodos HTTP

Cada método HTTP tem semântica específica e deve ser usado de acordo com as boas práticas RESTful, garantindo que as APIs sejam previsíveis e seguam os padrões estabelecidos.

### GET
- **Função**: Solicitar a representação de um recurso
- **Características**:
  - Apenas recupera dados
  - Pode ser armazenado em cache
  - Permanece no histórico do navegador
  - Pode ser marcado como favorito
  - Não deve alterar o estado do servidor

### POST
- **Função**: Enviar dados para serem processados pelo recurso identificado
- **Características**:
  - Frequentemente usado para criar novos recursos
  - Pode alterar o estado do servidor
  - Dados são enviados no corpo da requisição
  - Não é idempotente (várias requisições podem ter efeitos diferentes)

### PUT
- **Função**: Substituir todas as atuais representações do recurso de destino com os dados enviados
- **Características**:
  - Frequentemente usado para atualizar recursos existentes
  - Deve ser idempotente (várias requisições têm o mesmo efeito)
  - Envia a representação completa do recurso

### PATCH
- **Função**: Aplicar modificações parciais a um recurso
- **Características**:
  - Usado para atualizações parciais
  - Envia apenas as alterações, não o recurso completo
  - Nem sempre é idempotente

### DELETE
- **Função**: Remover o recurso especificado
- **Características**:
  - Remove um recurso do servidor
  - Pode ter um corpo de mensagem, mas geralmente não é usado
  - Deve ser idempotente

### HEAD
- **Função**: Igual ao GET, mas retorna apenas os headers, sem o corpo da resposta
- **Características**:
  - Útil para verificar se um recurso existe
  - Verificar metadados sem precisar transferir todo o conteúdo

### OPTIONS
- **Função**: Descrever as opções de comunicação para o recurso de destino
- **Características**:
  - Usado para descobrir quais métodos são suportados por um endpoint
  - Importante para CORS (Cross-Origin Resource Sharing)
