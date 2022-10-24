# Setup Your Database for TweetyTag


1. Setup repo
2. Create Aiven account
3. Create Service -> MySQL








1. Create Service in your Aiven Account:
![image](https://user-images.githubusercontent.com/77285384/197628419-de00d95b-0503-42f2-bf8c-949b2ff51963.png)



    Server Not Connected: 
    PrismaClientInitializationError: 
    Invalid `prisma.twitterHandle.findMany()` invocation in
    /Users/jennjunod/Desktop/code/tweetytag/index.js:14:60

      11 //     },
      12 //   })
      13 
    → 14   const alltwitterHandles = await prisma.twitterHandle.findMany(
    Can't reach database server at `mysql-21d62a09-teachjenntech-815a.aivencloud.com`:`20336`

    Please make sure your database server is running at `mysql-21d62a09-teachjenntech-815a.aivencloud.com`:`20336`.
        at RequestHandler.handleRequestError (/Users/jennjunod/Desktop/code/tweetytag/node_modules/@prisma/client/runtime/index.js:30879:13)
        at RequestHandler.request (/Users/jennjunod/Desktop/code/tweetytag/node_modules/@prisma/client/runtime/index.js:30856:12)
        at async PrismaClient._request (/Users/jennjunod/Desktop/code/tweetytag/node_modules/@prisma/client/runtime/index.js:31836:16)
        at async main (/Users/jennjunod/Desktop/code/tweetytag/index.js:14:33) {
      clientVersion: '4.5.0',
      errorCode: undefined
    }

Log into your Aiven Account to check if the database is running. 
![image](https://user-images.githubusercontent.com/77285384/197626503-a8fa9ca0-b7a0-4f66-bcdd-ad0f9c1a7e95.png)
If the database is not running, click into the database and click "Power on Service"
![image](https://user-images.githubusercontent.com/77285384/197627232-d55ce2bb-1b6e-4337-b1b6-5060b37e2bba.png)
