# Prisma

Referência [Rocketseat](https://www.youtube.com/watch?v=oWKuJnrceS8)

## Instalando oPrisma

```
yarn add prisma -D
```

```
yarn add @prisma/client
```

## Instalação

```cmd
npx prisma init
```

Esse comando irá criar uma pasta chamada **prisma** com os dados do schema do banco de dados.

Irá criar um arquivo **.env** ou adicionará uma variável ao arquivo .env já existente com informação de conexão do banco de dados.

```
DATABASE_URL="postgresql://johndoe:randompassword@localhost:5432/mydb?schema=public"
```

## Instalando a imagem docker

```
docker run --name treno -p 5435:5432 -d -e POSTGRES_PASSWORD=docker -e POSTGRES_USER=docker -e POSTGRES_DB=treno postgres:13.2
```

## Ajustando a conexão do Prisma
```
DATABASE_URL="postgresql://docker:docker@localhost:5435/treno?schema=public"
```

## Instalando extensão do Prisma para vscode

Procurar pela extensão **Prisma** highlights, etc...

# Configurações de Tabelas

Toda a criação e alteração nas tabelas do banco de dados é realizado no arquivo de schema do prisma. Diferentemente de outras lib como o sequelize que é precisa criar a migration para que seja executado no banco de dados, no Prisma a migration é gerada automáticamente após alterar o arquivo de schema.

## Criando uma tabela de Usuários

Abra a arquivo **schema.prisma** de dentro da pasta **prisma** e adicione a seguinte informação:

```
model User {
  id String @id
  name String
  email String
  password String
  lastAccess DateTime?
  createAt DateTime
  updateAt DateTime?
}
```

> Referência [Prisma](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model)

Após a alteração, salve o arquivo e execute o comando
```
yarn prisma migrate dev
```

Após executar o comando, na linha comando irá perguntar qual o nome da migrations deverá ser usado. Informe **create-users**

Será criado a pasta **migrations** dentro da pasta **prisma** com o arquivo gerado automaticamente

