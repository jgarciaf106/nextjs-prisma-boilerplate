# Fullstack  Next.js, NextAuth.js and Prism

## Set up Prisma and connect your PostgreSQL database
`$ npm install prisma --save-dev`
`$ npx prisma init`

## .env file changes
`DATABASE_URL="heroku postgresql db link"`

## Create your database schema with Prisma on schema.prisma file
```
model Post {
  id        Int     @default(autoincrement()) @id
  title     String
  content   String?
  published Boolean @default(false)
  author    User?   @relation(fields: [authorId], references: [id])
  authorId  Int?
}
```

### Push models to db
`$ npx prisma db push`

## Open Prisma Studio, a GUI for modifying your database.
`$ npx prisma studio`

## Install and generate Prisma Client
### Before you can access your database from Next.js using Prisma, you first need to install Prisma Client in your app
`$ npm install @prisma/client`

### Because Prisma Client is tailored to your own schema, you need to update it every time your Prisma schema file is changing by running the following command
`$ npx prisma generate`

## You'll use a single PrismaClient instance that you can import into any file where it's needed.
`$ mkdir lib && touch lib/prisma.ts`

### Now, add the following code to this file

```
// lib/prisma.ts
import { PrismaClient } from '@prisma/client'

let prisma: PrismaClient

if (process.env.NODE_ENV === 'production') {
  prisma = new PrismaClient()
} else {
  if (!global.prisma) {
    global.prisma = new PrismaClient()
  }
  prisma = global.prisma
}

export default prisma
```