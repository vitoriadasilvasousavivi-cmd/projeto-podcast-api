# Podcast API

API simples para listar e filtrar episódios de podcasts em vídeo. O projeto foi criado com Node.js puro e TypeScript, sem Express ou outro framework HTTP, para demonstrar a construção manual de rotas, controllers, services e repositories.

## Sobre o Projeto

A ideia do sistema é centralizar episódios de diferentes podcasts e permitir que um cliente, como um app ou site, consulte esses episódios por meio de uma API REST.

Atualmente a API possui duas funcionalidades principais:

- Listar todos os episódios cadastrados.
- Filtrar episódios pelo nome do podcast.

Os dados são lidos de um arquivo JSON local em `src/repositories/podcasts.json`.

## Tecnologias Utilizadas

- Node.js
- TypeScript
- TSX, para executar TypeScript em modo desenvolvimento
- TSUP, para gerar a build JavaScript em `dist`

## Estrutura do Projeto

```txt
src/
  app.ts
  server.ts
  controllers/
    podscasts-controller.ts
  models/
    podcast-model.ts
    podcast-transfer-model.ts
  repositories/
    podcasts-repository.ts
    podcasts.json
  routes/
    routes.ts
  services/
    filter-episodes-service.ts
    list-episodes-service.ts
  utils/
    content-type.ts
    http-methods.ts
    status-code.ts
```

## Como o Sistema Foi Criado

O projeto foi organizado em camadas:

- `server.ts`: cria o servidor HTTP usando o módulo nativo `http` do Node.js.
- `app.ts`: recebe as requisições e direciona para o controller correto de acordo com a rota.
- `routes.ts`: centraliza os caminhos da API.
- `controllers`: recebem a requisição, chamam os services e devolvem a resposta HTTP.
- `services`: aplicam a regra de negócio, como listar ou filtrar episódios.
- `repositories`: acessam a fonte de dados, que neste projeto é o arquivo `podcasts.json`.
- `models`: definem os tipos TypeScript usados pela aplicação.
- `utils`: guardam constantes reutilizáveis, como status HTTP, métodos HTTP e content types.

Essa separação deixa o projeto mais fácil de manter e evoluir. Por exemplo, futuramente o arquivo JSON pode ser substituído por um banco de dados sem precisar mudar toda a aplicação.

## Requisitos

- Node.js instalado
- npm instalado

O projeto foi verificado com:

```bash
node -v
npm -v
```

## Instalação

Clone o repositório e instale as dependências:

```bash
npm install
```

Crie ou mantenha o arquivo `.env` na raiz do projeto:

```env
PORT=3333
```

## Executando em Desenvolvimento

```bash
npm run start:dev
```

Servidor esperado:

```txt
servidor iniciado na porta 3333
```

Depois acesse:

```txt
http://localhost:3333/api/list
```

## Executando com Watch

Use este comando para reiniciar automaticamente durante alterações no código:

```bash
npm run start:watch
```

## Gerando Build

```bash
npm run dist
```

Esse comando gera os arquivos compilados na pasta `dist`.

## Executando a Versão Compilada

```bash
npm run start:dist
```

Esse comando gera a build e executa o arquivo `dist/server.js`.

## Rotas da API

### Listar Todos os Episódios

```http
GET /api/list
```

Exemplo:

```txt
http://localhost:3333/api/list
```

Resposta:

```json
[
  {
    "podcastName": "flow",
    "episode": "CBUM - Flow #319",
    "videoId": "pQSuQmUfS30",
    "categories": ["saúde", "esporte", "bodybuilder"]
  }
]
```

### Filtrar por Nome do Podcast

```http
GET /api/podcasts?p=flow
```

Exemplo:

```txt
http://localhost:3333/api/podcasts?p=flow
```

Retorna somente os episódios em que `podcastName` é igual ao valor enviado no parâmetro `p`.

## Exemplos de Teste

Com o servidor rodando, teste no navegador, Postman, Insomnia ou terminal:

```bash
curl http://localhost:3333/api/list
```

```bash
curl "http://localhost:3333/api/podcasts?p=flow"
```

## Scripts Disponíveis

```json
{
  "start:dev": "tsx --env-file=.env src/server.ts",
  "start:watch": "tsx watch --env-file=.env src/server.ts",
  "dist": "tsup src",
  "start:dist": "npm run dist && node --env-file=.env dist/server.js"
}
```

## Status do Projeto

Verificações realizadas:

- Checagem TypeScript com `npx tsc --noEmit`.
- Build com `npm run dist`.
- Execução da API compilada com `node --env-file=.env dist/server.js`.
- Teste da rota `/api/list`, retornando 4 episódios.
- Teste da rota `/api/podcasts?p=flow`, retornando 3 episódios.
- Teste da rota `/api/podcasts?p=venus`, retornando 1 episódio.

## Possíveis Melhorias Futuras

- Adicionar tratamento para rotas inexistentes com status `404`.
- Adicionar validação para parâmetros de busca.
- Corrigir ou padronizar nomes de arquivos e rotas, como `podscasts-controller.ts` e `ESPISODE`.
- Substituir o JSON local por um banco de dados.
- Criar testes automatizados.
- Adicionar campos como `cover` e `link` aos episódios.

