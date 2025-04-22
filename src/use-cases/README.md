📁 Explicação da Pasta use-cases
A pasta use-cases é onde fica a regra de negócio principal da aplicação. Aqui moram os “casos de uso”, ou seja, as ações que o sistema sabe fazer — como registrar um usuário, fazer login, etc.

📄 Arquivo: use-cases/register-user.ts
Esse arquivo implementa o caso de uso para registrar um novo usuário no sistema.

🔍 Objetivo
Garantir que:

Um usuário com o mesmo e-mail não seja registrado duas vezes;

A senha seja armazenada de forma segura (com hash);

O novo usuário seja salvo no banco de dados.

📦 Importações
ts
Copy
Edit
import { UsersRepository } from '@/repositories/users-repository'
import { User } from '@prisma/client'
import { hash } from 'bcryptjs'
import { UserAlreadyExistsError } from './errors/user-alredy-exist'
UsersRepository: Interface do repositório de usuários (o "contrato" que permite usar Prisma ou in-memory).

hash: Função do bcryptjs para criptografar senhas.

UserAlreadyExistsError: Classe de erro personalizada (vamos explicar abaixo).

✨ A classe RegisterUserUseCase
ts
Copy
Edit
export class RegisterUserUseCase {
  constructor(private userRepository: UsersRepository) { }
Usa injeção de dependência para receber o repositório (pode ser o real ou o fake).

Isso permite testar a lógica sem precisar de banco de dados de verdade.

📥 Parâmetros esperados
ts
Copy
Edit
interface RegisterUserRequest {
  name: string
  email: string
  password: string
}
O que o sistema precisa para cadastrar alguém.

📤 Retorno da função
ts
Copy
Edit
interface RegisterUserResponse {
  user: User
}
Retorna o usuário recém-criado.

🧠 Método principal: execute()
ts
Copy
Edit
async execute({ name, email, password }: RegisterUserRequest)
O que ele faz:

Criptografa a senha com bcryptjs.

Verifica se já existe um usuário com o mesmo e-mail.

Se já existir, lança um erro: UserAlreadyExistsError.

Caso contrário, cria o novo usuário no repositório.

Retorna o novo usuário.

📁 Subpastas da pasta use-cases
🧱 errors/
Contém erros personalizados, como:

ts
Copy
Edit
export class UserAlreadyExistsError extends Error {
  constructor() {
    super('User already exists.')
  }
}
Esses erros ajudam a entender melhor o que deu errado, e permitem retornar mensagens específicas para o usuário.

🏭 factories/
As factories são funções que ajudam a criar os casos de uso prontos para uso, injetando as dependências certas. Exemplo:

ts
Copy
Edit
import { RegisterUserUseCase } from '../register-user'
import { PrismaUsersRepository } from '@/repositories/prisma/prisma-users-repository'

export function makeRegisterUserUseCase() {
  const repository = new PrismaUsersRepository()
  return new RegisterUserUseCase(repository)
}
Assim você centraliza a criação e facilita a manutenção.

🧪 tests/
Aqui ficam os testes automatizados dos casos de uso. Normalmente usam o in-memory repository para simular o banco e testar o comportamento da lógica.

✅ Resumo

Item	Função
RegisterUserUseCase	Registra novos usuários com senha criptografada
errors/	Define mensagens de erro claras e reutilizáveis
factories/	Cria os casos de uso com as dependências corretas
tests/	Garante que os casos de uso funcionam como esperado
