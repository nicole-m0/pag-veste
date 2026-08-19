# Pagvest

## Desenvolvimento local

No diretório `pagvest`, instale as dependências e inicie o servidor:

```bash
npm ci
npm run dev
```

Abra [http://localhost:3000](http://localhost:3000). Use `.env.local` para valores locais; o `.env.example` documenta os nomes das variáveis e não contém segredos.

## Deploy na Vercel

Ao importar o repositório na Vercel, selecione **Root Directory** como `pagvest`. O framework e os comandos padrão serão detectados automaticamente:

- Install Command: `npm ci`
- Build Command: `npm run build`
- Output Directory: deixe em branco

Cadastre estes segredos na Vercel:

| Variável | Ambientes | Uso |
| --- | --- | --- |
| `DATABASE_URL` | Production, Preview e Development | URL externa do PostgreSQL no Railway, com SSL quando fornecido pelo Railway. |
| `CLOUDINARY_CLOUD_NAME` | Production, Preview e Development | Conta Cloudinary, uso exclusivo no servidor. |
| `CLOUDINARY_API_KEY` | Production, Preview e Development | Credencial Cloudinary, uso exclusivo no servidor. |
| `CLOUDINARY_API_SECRET` | Production, Preview e Development | Segredo Cloudinary, uso exclusivo no servidor. |

Não use `NEXT_PUBLIC_` para nenhuma dessas variáveis. Se previews precisarem de outro banco ou conta Cloudinary, informe valores próprios apenas em **Preview**. Caso contrário, os mesmos valores podem ser usados nos três ambientes.

## Estado atual das integrações

Este repositório ainda não contém Prisma, `schema.prisma`, migrations, rotas `app/api`, dependências Cloudinary ou código de upload. Portanto, nenhuma das variáveis acima é lida pelo código atual e não há CRUD para testar no Postman neste commit. Quando esses módulos forem adicionados, o Prisma Client deve ser gerado no build (por exemplo, `prisma generate && next build`), o `PrismaClient` deve ser reutilizado em um módulo singleton no servidor, e uploads devem enviar o arquivo diretamente ao Cloudinary, persistindo somente URL e/ou `public_id` no PostgreSQL.

Não há necessidade de trocar o banco do Railway. Para a Vercel conectar, use a URL pública/external fornecida pelo Railway e garanta que o PostgreSQL aceite conexões externas; não use uma URL privada/interna do Railway. Não execute migrations automaticamente no build da Vercel: aplique-as de forma controlada com `prisma migrate deploy` quando Prisma existir.
