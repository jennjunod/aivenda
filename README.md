# Setup Your Database for TweetyTag

Before TweetyTag can auto post the users that have registered to be tagged in TwitterSpace reminders, the database must be setup. We will be using [Prisma ORM](https://www.prisma.io/) as the tool to connect the backend of node to use MySQL with Aiven. 

1. [Create database with Aiven](https://github.com/jennjunod/aivenda/blob/main/README.md#1-create-service-in-your-aiven-account)
2. [Prisma Client for database access](https://github.com/jennjunod/aivenda/blob/main/README.md#2-using-the-relational-databases-article-from-prisma-create-project-setup)(Using JavaScript and MySQL)
3. [Write to database](https://github.com/jennjunod/aivenda/blob/main/README.md#3-create-a-new-file-named-writejs-to-add-the-following-code-to-it)
4. [Read from database](https://github.com/jennjunod/aivenda/blob/main/README.md#4-create-a-new-file-named-readjs-to-add-the-following-code-to-it)


## 1. Create Service in your Aiven Account:
* [Getting started with Aiven for MySQL](https://docs.aiven.io/docs/products/mysql/get-started.html#getting-started-with-aiven-for-mysql)

![image](https://user-images.githubusercontent.com/77285384/197628419-de00d95b-0503-42f2-bf8c-949b2ff51963.png)


### Once the database shows running:

![image](https://user-images.githubusercontent.com/77285384/197630786-571055ce-43c0-427e-b6f3-62b31eeefe45.png)


### The connection information will become avaiable:

![image](https://user-images.githubusercontent.com/77285384/197631205-0848f9f7-5052-4f8c-b26f-7111f63b5387.png)


## 2. Using the [Relational databases](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-node-mysql) article from Prisma, Create project setup
* Continue by [connecting your database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-node-mysql), which with Prisma, the default database provider is Postgresql, make sure to change the provider to MySQL.
    datasource db {
      provider = "postgresql"
      url      = env("DATABASE_URL")
    }
Change to: 
    datasource db {
      provider = "mysql"
      url      = env("DATABASE_URL")
    }
* When the Prisma Client Setup has been completed, files to create the write and read [queries to the database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/querying-the-database-node-mysql). 


## 3. Create a new file named `write.js` to add the following code to it:

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
* To ensure the database can write a query, run in your terminal, `node write.js`


## 4. Create a new file named `read.js` to add the following code to it:

        const { PrismaClient } = require('@prisma/client')

        const prisma = new PrismaClient()

        async function main() {
            // ... you will write your Prisma Client queries here
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
          
          
* To ensure the database can read the queary, run `node read.js` in your terminal. 

### Next blog series, we will review how to setup your Prisma Client to allow front end input your database and in the final post, we will share how to connect to the Twitter API. 

Follow the TweetyTag journey by following Jenn on:
- [GitHub](https://github.com/jennjunod)
- [Twitter](https://twitter.com/JennJunod)
- [LinkedIn](https://www.linkedin.com/in/jennjunod/)
- [DevTo](https://dev.to/jennjunod)
