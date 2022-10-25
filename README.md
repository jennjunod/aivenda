# Setup Your Database for TweetyTag

Before TweetyTag can auto post the users that have registered to be tagged in TwitterSpace reminders, the database must be setup. We will be using [Prisma ORM](https://www.prisma.io/) as the tool to connect a Node backend to MySQL with Aiven. 

1. [Create database with Aiven](https://github.com/jennjunod/aivenda/blob/main/README.md#1-create-service-in-your-aiven-account)
2. [Prisma Client for database access](https://github.com/jennjunod/aivenda/blob/main/README.md#2-using-the-relational-databases-article-from-prisma-create-project-setup)
3. [Read/Write Query](https://github.com/jennjunod/aivenda/blob/main/README.md#3-create-a-new-file-named-indexjs)
4. [Write to database](https://github.com/jennjunod/aivenda/blob/main/README.md#4-create-a-new-file-named-writejs)
5. [Read from database](https://github.com/jennjunod/aivenda/blob/main/README.md#5-create-a-new-file-named-readjs)

## 1. Create Service in your Aiven Account:

See [Getting started with Aiven for MySQL](https://docs.aiven.io/docs/products/mysql/get-started.html#getting-started-with-aiven-for-mysql).

![image](https://user-images.githubusercontent.com/77285384/197628419-de00d95b-0503-42f2-bf8c-949b2ff51963.png)

Once the database is running you will see the following:

![image](https://user-images.githubusercontent.com/77285384/197630786-571055ce-43c0-427e-b6f3-62b31eeefe45.png)

The connection information will become avaiable:

![image](https://user-images.githubusercontent.com/77285384/197631205-0848f9f7-5052-4f8c-b26f-7111f63b5387.png)

## 2. Using the [Relational databases](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-node-mysql) article from Prisma, Create project setup

Continue by [connecting your database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-node-mysql). Prisma has the default database provider set to `postgresql` so make sure to change the provider to `mysql`.

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```
        
Change to: 

```prisma
datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

Include the following for our models.

```prisma
model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  title     String   @db.VarChar(255)
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
}

model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
  posts   Post[]
  profile Profile?
}
```

Now that the Prisma Client is setup, we'll create a file to [read and write to the database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/querying-the-database-node-mysql). 

## 3. Create a new file named `index.js`:

This will allow you to read and write from the same file.

```js
const { PrismaClient } = require('@prisma/client')

const prisma = new PrismaClient()

async function main() {
  await prisma.user.create({
    data: {
      name: 'Alice',
      email: 'alice@prisma.io',
      posts: {
        create: { title: 'Hello World' },
      },
      profile: {
        create: { bio: 'I like turtles' },
      },
    },
  })

  const allUsers = await prisma.user.findMany({
    include: {
      posts: true,
      profile: true,
    },
  })
  console.dir(allUsers, { depth: null })
}

main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
```

To ensure the database can write a query, run the following command in your terminal:

```bash
node index.js
```

## 4. Create a new file named `write.js`:

To query the DB seperately, add the following to `write.js`: 

```js
const { PrismaClient } = require('@prisma/client')

const prisma = new PrismaClient()

async function main() {
  await prisma.user.create({
    data: {
      name: 'Alice2',
      email: 'alice2@prisma.io',
      posts: {
        create: { title: 'Hello World' },
      },
      profile: {
        create: { bio: 'I like turtles' },
      },
    },
  })
}

main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
```

To ensure the database can write a query, run the following command in your terminal:

```bash
node write.js
```

## 5. Create a new file named `read.js`:

```js
const { PrismaClient } = require('@prisma/client')

const prisma = new PrismaClient()

async function main() {
    const allUsers = await prisma.user.findMany({
      include: {
        posts: true,
        profile: true,
      },
    })
    console.dir(allUsers, { depth: null })
  }

main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
```

To ensure the database can read the query, run `node read.js` in your terminal.

```bash
node read.js
```

Success of the `node read.js` command will look like this:

```js
[
  {
    id: 1,
    email: 'alice@prisma.io',
    name: 'Alice',
    posts: [
      {
        id: 1,
        createdAt: 2022-10-24T23:06:12.491Z,
        updatedAt: 2022-10-24T23:06:12.491Z,
        title: 'Hello World',
        content: null,
        published: false,
        authorId: 1
      }
    ],
    profile: { id: 1, bio: 'I like turtles', userId: 1 }
  }
]
```

In the next article of this blog series, we will review how to setup your Prisma Client to allow front end input into your database. In the final post, we will share how to connect to the Twitter API.

Follow the TweetyTag journey by following Jenn on:
- [GitHub](https://github.com/jennjunod)
- [Twitter](https://twitter.com/JennJunod)
- [LinkedIn](https://www.linkedin.com/in/jennjunod/)
- [DevTo](https://dev.to/jennjunod)
