📁 Explicação da Pasta repositories
Essa pasta cuida da comunicação com o banco de dados. Mas mais importante ainda: ela organiza essa comunicação de forma flexível, usando os princípios do SOLID, especialmente o Princípio da Inversão de Dependência (D).

📄 Arquivo: repositories/users-repository.ts
ts
Copy
Edit
import { Prisma, User } from '@prisma/client'

export interface UsersRepository {
  create(data: Prisma.UserCreateInput): Promise<User>
  findByEmail(email: string): Promise<User | null>
}
Esse arquivo define uma interface, ou seja, um contrato que diz o que um "repositório de usuários" deve saber fazer. Aqui, ele exige dois métodos:

create(data): Cria um novo usuário no banco de dados.

findByEmail(email): Procura um usuário pelo e-mail.

⚠️ Importante: essa interface não implementa nada ainda. Ela só descreve o que qualquer "repositório de usuários" deve saber fazer.

🤔 Por que usar uma interface?
Porque isso nos permite trocar a implementação do repositório sem mudar o resto do sistema.

Exemplo:

No mundo real: usamos um repositório com o Prisma que conversa com o banco.

Em testes: usamos um repositório "falso" (em memória), que simula o banco sem realmente acessá-lo.

📄 Arquivo: repositories/in-memory/in-memory-users-repository.ts
ts
Copy
Edit
import { User, Prisma } from '@prisma/client'
import { UsersRepository } from '../users-repository'
import { randomUUID } from 'node:crypto'

export class InMemoryUsersRepository implements UsersRepository {
  private users: User[] = []

  async create(data: Prisma.UserCreateInput): Promise<User> {
    const user = {
      id: randomUUID(),
      name: data.name,
      email: data.email,
      password_hash: data.password_hash,
      created_at: new Date(),
    }

    this.users.push(user)

    return user
  }

  async findByEmail(email: string): Promise<User | null> {
    const user = this.users.find(user => user.email === email)
    return user || null
  }
}
Essa é uma implementação da interface, mas funciona totalmente na memória (sem banco de dados).

🧠 O que esse código faz?
users: Um array local que simula uma "tabela" de usuários.

create(data): Cria um novo usuário com um ID aleatório e salva no array.

findByEmail(email): Procura no array por um usuário com aquele e-mail.

🧪 Para que serve isso?
Para testes automáticos!
Assim podemos testar a lógica do sistema sem depender de um banco real — isso deixa os testes mais rápidos e mais confiáveis.

🔁 Comparando as duas abordagens

Interface	Implementação com Prisma	Implementação em memória
Define o que deve ser feito	Salva no banco real	Salva num array local
Produção real	✅	❌
Testes rápidos	❌	✅