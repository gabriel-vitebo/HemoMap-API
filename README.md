🐳 docker-compose.yml
Esse arquivo serve para subir o PostgreSQL com um comando só, sem precisar instalar nada manualmente.

📄 Exemplo comum:
yaml
Copy
Edit
version: '3'

services:
  postgres:
    image: postgres
    container_name: db
    restart: always
    ports:
      - '5432:5432'
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: solid
    volumes:
      - pgdata:/data/postgres

volumes:
  pgdata:
🧠 O que ele faz?
Sobe um banco PostgreSQL com:

Usuário: postgres

Senha: postgres

Banco: solid

Mapeia a porta 5432 (porta padrão do Postgres).

Usa um volume para salvar os dados do banco, mesmo que o container pare.

🚀 Como usar?
bash
Copy
Edit
docker-compose up -d
Isso inicia o banco de dados em segundos.

📦 package.json
Esse é o arquivo principal de qualquer projeto Node.js. Ele guarda informações sobre:

Nome do projeto

Scripts para rodar comandos

Dependências usadas

📄 Exemplo:
json
Copy
Edit
{
  "name": "solid-api",
  "version": "1.0.0",
  "scripts": {
    "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "lint": "eslint ."
  },
  "dependencies": {
    "@prisma/client": "^5.0.0",
    "bcryptjs": "^2.4.3",
    "dotenv": "^16.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "ts-node-dev": "^2.0.0",
    "prisma": "^5.0.0"
  }
}
📌 O que cada parte faz?
scripts: Comandos úteis que você roda com npm run.

dependencies: Pacotes que o projeto usa em produção.

devDependencies: Pacotes que só são usados durante o desenvolvimento.

📁 .gitignore
Esse arquivo diz ao Git quais arquivos NÃO devem ser enviados para o repositório.

📄 Exemplo comum:
bash
Copy
Edit
node_modules
.env
dist
.generated
❌ Por que ignorar esses?

Pasta/Arquivo	Motivo
node_modules/	Pasta gigante, pode ser recriada com npm install
.env	Contém senhas e dados sensíveis
dist/	Arquivos compilados, não precisam estar no Git
.generated/	Códigos gerados automaticamente (ex: Prisma client)
✅ Resumo dos 3 arquivos

Arquivo	Função Principal
docker-compose.yml	Sobe o PostgreSQL com Docker
package.json	Gerencia dependências e scripts do projeto
.gitignore	Evita que arquivos desnecessários ou sensíveis entrem no Git