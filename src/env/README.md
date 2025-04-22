📄 Explicação do Arquivo env.ts
Esse arquivo é responsável por carregar e validar as variáveis de ambiente da aplicação. Ele garante que a aplicação só funcione se essas variáveis estiverem configuradas corretamente.

🤔 O que são variáveis de ambiente?
São valores configuráveis que ficam fora do código. Exemplo: em qual porta o servidor vai rodar, se está em ambiente de desenvolvimento ou produção, etc.
Essas variáveis geralmente ficam em um arquivo .env e são úteis para personalizar o comportamento da aplicação sem alterar o código.

📦 Importações
ts
Copy
Edit
```
import 'dotenv/config'
import { z } from 'zod'
```
- dotenv/config: Carrega automaticamente o conteúdo do arquivo .env para dentro do process.env.

- zod: Biblioteca para validação de dados. Aqui, usamos ela para garantir que as variáveis de ambiente estão no formato certo.

✅ Esquema de validação (envSchema)
ts
Copy
Edit
```
const envSchema = z.object({
  NODE_ENV: z.enum(['dev', 'test', 'production']).default('dev'),
  PORT: z.coerce.number().default(3333),
})
```
NODE_ENV: Deve ser uma dessas três opções — 'dev' (desenvolvimento), 'test' (teste), ou 'production' (produção). Se não for definido, assume 'dev'.

PORT: Deve ser um número. Se for passado como string (ex: '3000'), o z.coerce.number() converte para número. Se não for definido, assume 3333.

🔍 Validação das variáveis
ts
Copy
Edit
const _env = envSchema.safeParse(process.env)
Aqui o Zod verifica se as variáveis de ambiente estão corretas de acordo com o envSchema.

❌ Em caso de erro
ts
Copy
Edit
if (_env.success === false) {
  console.error('❌ Invalid environment variables', _env.error.format())
  throw new Error('❌ Invalid environment variables')
}
Se alguma variável estiver incorreta ou ausente, o programa mostra um erro no console e interrompe a execução. Isso evita que o sistema rode com configurações inválidas.

✅ Exportando as variáveis válidas
ts
Copy
Edit
export const env = _env.data
Aqui exportamos as variáveis já validadas para que o restante do código possa usá-las com segurança.

🧠 Em resumo
Esse arquivo serve para:

Carregar as variáveis do .env;

Garantir que elas estejam corretas;

Interromper o sistema caso algo esteja errado.

Assim, evitamos problemas como rodar o servidor na porta errada ou no ambiente errado (ex: subir ambiente de testes como se fosse produção).