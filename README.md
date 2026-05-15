# Projeto API IoT - Monitoramento de Temperatura e Umidade

Este projeto consiste em uma API REST desenvolvida em Node.js para gerenciar dados provenientes de sensores de temperatura e umidade, simulando um cenário real de IoT.

## PARTE 1 — PESQUISA CONCEITUAL

### 1.1) O que é uma API?
API é a sigla para Application Programming Interface (Interface de Programação de Aplicações). Imagine que ela seja uma ponte ou um tradutor que permite que dois softwares diferentes "conversem" entre si, sem que um precise conhecer os detalhes internos do outro. Ela define regras e padrões para que essa comunicação seja segura e eficiente.

Uma analogia prática é o **Cardápio de uma Cafeteria**. Você (o cliente/front-end) olha o cardápio (API) e faz um pedido específico. Você não entra na cozinha para ligar a máquina de café ou moer os grãos; você entrega seu pedido ao atendente, que leva a instrução para a cozinha (servidor) e traz seu café pronto. A API é esse cardápio e o protocolo de pedido.

### 1.2) O que é REST?
REST (Representational State Transfer) é um conjunto de "boas práticas" ou restrições de arquitetura para a criação de APIs que utilizam o protocolo HTTP. Uma API que segue esses princípios é chamada de RESTful. O foco do REST é tratar tudo como um "recurso" (no nosso caso, os dados dos sensores) que pode ser acessado através de URLs amigáveis.

Ser RESTful significa que a comunicação é sem estado (stateless), ou seja, cada requisição do cliente para o servidor deve conter todas as informações necessárias para ser entendida. Isso torna o sistema mais escalável e fácil de manter, pois padroniza a forma como os dados são enviados e recebidos na web.

### 1.3) O que é CRUD?
CRUD é um acrônimo para as quatro operações básicas de qualquer sistema de dados. No nosso código, implementamos todas elas:
- **C (Create/Criar):** Usamos o método **POST** para adicionar novas leituras de sensores ao nosso histórico.
- **R (Read/Ler):** Usamos o método **GET** para buscar todas as leituras ou uma leitura específica por ID.
- **U (Update/Atualizar):** Usamos o método **PUT** para modificar os dados de uma leitura que já existe no sistema.
- **D (Delete/Excluir):** Usamos o método **DELETE** para remover um registro específico do nosso histórico de sensores.

### 1.4) HTTP e Status Codes
O HTTP (Hypertext Transfer Protocol) é o protocolo fundamental da internet. Ele funciona em um modelo de requisição (cliente pede) e resposta (servidor entrega). Para cada resposta, o servidor envia um código numérico que indica o que aconteceu.

| Código | Nome | Significado | Quando aparece no nosso projeto |
|:---:|:---:|:---:|:---:|
| 200 | OK | Sucesso total na requisição. | Ao listar os dados (GET) ou atualizar (PUT). |
| 201 | Created | Recurso criado com sucesso. | Após enviar novos dados com sucesso (POST). |
| 400 | Bad Request | Erro no cliente (dados faltando). | Quando tentamos dar POST sem temperatura ou hora. |
| 404 | Not Found | Recurso não encontrado. | Ao buscar, excluir ou editar um ID que não existe. |
| 500 | Internal Server Error | Erro genérico no servidor. | Se houver uma falha crítica no código do Node.js. |
| 204 | No Content | Sucesso, mas sem corpo na resposta. | Poderia ser usado no DELETE (embora usemos 200 com mensagem). |

### 1.5) JSON: O Formato de Dados
JSON (JavaScript Object Notation) é um formato leve de troca de dados, fácil de ler para humanos e fácil de processar para máquinas. Ele se baseia em chaves e valores. Escolhemos o JSON porque ele é o padrão da indústria e funciona nativamente com JavaScript.

**Exemplo de um dado da nossa API:**
```json
{
  "id": 1,
  "temperatura": 30,
  "umidade": 40,
  "hora": "10:00"
}
````
## PARTE 2 — DOCUMENTAÇÃO DOS ENDPOINTS/ROTAS
1. Listar Todos os Dados

    Rota: GET /api/dados

    Descrição: Retorna a lista completa de leituras armazenadas.

    Resposta Sucesso: Status 200 | Body: Lista de objetos JSON.
---
<img width="1540" height="908" alt="image" src="https://github.com/user-attachments/assets/ea9a16c8-baef-4920-a1c0-37c2de1b1725" />

---
2. Buscar por ID

    Rota: GET /api/dados/:id

    Parâmetros: id (na URL)

    Resposta Erro: Status 404 | {"mensagem": "ID não encontrado!"}
---
<img width="1539" height="910" alt="image" src="https://github.com/user-attachments/assets/f4732008-bb0c-442c-85bd-75771ec51085" />

---
3. Inserir Novo Dado

    Rota: POST /api/dados

    Body: {"temperatura": 22, "umidade": 60, "hora": "14:00"}

    Resposta Sucesso: Status 201 | {"mensagem": "Dados enviados com sucesso!", "dados": {...}}
---
<img width="1537" height="908" alt="image" src="https://github.com/user-attachments/assets/2c878d06-adf1-4228-b576-1a26914ce86a" />

---
4. Atualizar Dado

    Rota: PUT /api/dados/:id

    Body: Novos valores de temperatura/umidade.

    Descrição: Substitui os dados de um ID existente.
---
<img width="1538" height="909" alt="image" src="https://github.com/user-attachments/assets/ccb94b9f-dddf-4750-8562-2bd2a5a1ac8e" />

---
## PARTE 3 — DIAGRAMA DA ARQUITETURA

ESP32 (Sensores) --[POST/HTTP]--> API (Node.js) <--> Banco de Dados (Memória)
API --[JSON/HTTP]--> Postman/Client]

## PARTE 4 — COMO RODAR E REFLEXÃO
4.1) Como Rodar

    1. Certifique-se de ter o Node.js instalado (v14 ou superior).

    2. No terminal, na pasta do projeto, instale as dependências:
---
<img width="1571" height="264" alt="image" src="https://github.com/user-attachments/assets/648b847e-a88b-48f3-8c84-bf81c90f9999" />

---
    3. Iniice o servidor:
---
<img width="1573" height="286" alt="image" src="https://github.com/user-attachments/assets/ca4a4b64-6b6a-41d5-8d1d-8a1e76f9d77a" />

---
    4. O servidor estará rodando em http://localhost:3000. Teste usando o Postman fazendo um GET nesta URL.

---

4.2) Tecnologias Usadas

    Node.js: Ambiente de execução do JavaScript no servidor.

    Express: Framework para gerenciar rotas e requisições HTTP de forma simples.

    Cors: Middleware para permitir que outras aplicações acessem nossa API.

    Nodemon (opcional): Para reiniciar o servidor automaticamente durante o desenvolvimento.



