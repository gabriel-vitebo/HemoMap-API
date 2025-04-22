📁 Prisma, PostgreSQL e Migrations — Explicação Didática
📄 Arquivo: prisma/schema.prisma
Esse é o coração do Prisma: o arquivo onde a gente descreve como é o banco de dados.

🔍 O que é o Prisma?
O Prisma é uma ferramenta que conecta o Node.js com o banco de dados. Ele ajuda a:

Criar tabelas com código;

Consultar e salvar dados com segurança;

Atualizar a estrutura do banco com facilidade.

🛠 Bloco generator
prisma
Copy
Edit
generator client {
  provider = "prisma-client-js"
  output   = "../generated/prisma"
}
Diz ao Prisma que vamos usar o cliente JavaScript.

Ele gera um código automático para acessar o banco.

O resultado vai para a pasta generated/prisma.

🗄 Bloco datasource
prisma
Copy
Edit
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
Define qual banco estamos usando (nesse caso, PostgreSQL).

A url é pega do arquivo .env — algo assim:

ini
Copy
Edit
DATABASE_URL="postgresql://usuario:senha@localhost:5432/nome-do-banco"
🧩 Modelo User
prisma
Copy
Edit
model User {
  id        String   @id @default(uuid())
  name      String?
  email     String?  @unique
  password  String?
  createdAt DateTime @default(now())

  @@map("users")
}
Esse bloco define a tabela de usuários no banco, com os seguintes campos:


Campo	Tipo	Descrição
id	String	Chave primária gerada automaticamente (UUID).
name	String?	Nome do usuário (pode ser nulo).
email	String?	E-mail do usuário (pode ser nulo, mas deve ser único).
password	String?	Senha do usuário.
createdAt	DateTime	Data de criação automática.
@@map("users")		Garante que no banco a tabela se chame users (não User).
🔄 O que são Migrations?
As migrations (ou migrações) são arquivos que servem para:

Registrar mudanças no banco de dados (como adicionar colunas, criar tabelas, etc.);

Garantir que todos os ambientes (dev, teste, produção) estejam com a estrutura do banco igual.

🧪 Como usar na prática?
Alguns comandos úteis com Prisma + PostgreSQL:

Criar a migration:

bash
Copy
Edit
npx prisma migrate dev --name create-users
Cria a estrutura no banco e registra essa "mudança" com um nome (create-users).

Visualizar o banco:

bash
Copy
Edit
npx prisma studio
Abre um painel web para visualizar e editar os dados no banco.

Gerar o client Prisma:

bash
Copy
Edit
npx prisma generate
Gera o código do Prisma para poder usar no projeto.

📌 Resumo

Parte	Função
schema.prisma	Descreve a estrutura das tabelas do banco
generator	Define que será gerado um cliente para acessar o banco via código
datasource	Configura o tipo de banco (PostgreSQL) e de onde vem a URL
User	Modelo da tabela de usuários
Migrations	Histórico de alterações no banco, aplicadas com comandos do Prisma