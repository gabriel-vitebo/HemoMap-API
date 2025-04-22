📄 Explicação do Arquivo lib/prisma.ts
Esse arquivo é responsável por criar e configurar a conexão com o banco de dados usando o Prisma, uma ferramenta que facilita o trabalho com bancos de dados no Node.js.

📦 Importações
ts
Copy
Edit
import { env } from '@/env'
import { PrismaClient } from '@prisma/client'
@/env: Importa as variáveis de ambiente validadas que explicamos no arquivo anterior.

PrismaClient: É a classe principal do Prisma. Com ela, conseguimos fazer consultas no banco de dados de forma simples e segura.

🔧 Criação do Cliente Prisma
ts
Copy
Edit
export const prisma = new PrismaClient({
  log: env.NODE_ENV === 'dev' ? ['query'] : [],
})
Aqui estamos:

Criando uma instância do Prisma (new PrismaClient()).

Passando uma configuração chamada log:

Se estivermos no ambiente de desenvolvimento (NODE_ENV === 'dev'), ele vai mostrar no terminal todas as queries (consultas) feitas no banco de dados.

Em qualquer outro ambiente (como produção), ele não mostra nada, para não poluir o terminal e evitar vazamento de informações sensíveis.

🧠 Em resumo
Esse arquivo:

Cria o "cliente" que será usado para falar com o banco de dados;

Configura o Prisma para mostrar ou não as queries no terminal, dependendo do ambiente;

Exporta essa instância (prisma) para ser usada em qualquer lugar do projeto, como nos controllers, services, etc.

💡 Exemplo de uso
Em outro arquivo, poderíamos usar assim:

ts
Copy
Edit
import { prisma } from '@/lib/prisma'

const users = await prisma.user.findMany()
Isso faria uma consulta no banco e traria todos os usuários da tabela user.