# Setup Your Database for TweetyTag

Before TweetyTag can auto post the users that have registered to be tagged in TwitterSpace reminders, the database must be setup. We will be using [Prisma ORM](https://www.prisma.io/) as the tool to connect the backend of node to use MySQL with Aiven. 

1. Create database with Aiven
2. Prisma Client for database access (Using JavaScript and MySQL)
3. Write to database
4. Read from database


Create Service in your Aiven Account:
![image](https://user-images.githubusercontent.com/77285384/197628419-de00d95b-0503-42f2-bf8c-949b2ff51963.png)

Once the database shows running
![image](https://user-images.githubusercontent.com/77285384/197630786-571055ce-43c0-427e-b6f3-62b31eeefe45.png)

The connection information will become avaiable
![image](https://user-images.githubusercontent.com/77285384/197631205-0848f9f7-5052-4f8c-b26f-7111f63b5387.png)

Using the [Relational databases](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-node-mysql) article from Prisma, Create project setup

Continue by [connecting your database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-node-mysql), which with Prisma, the default database provider is Postgresql, make sure to change the provider to MySQL.

When the Prisma Client Setup has been completed, files to create the write and read [queries to the database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/querying-the-database-node-mysql). 

Create a new file named write.js to add the following code to it

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

Create a new file named read.js to add the following code to it"

        const { PrismaClient } = require('@prisma/client')

        const prisma = new PrismaClient()

        async function main() {
          // ... you will write your Prisma Client queries here
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
          
          
To ensure the database can write a query, run in your terminal, node read.js
To ensure the database can read the queary, run node write.js in your terminal. 
