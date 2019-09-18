## SQS
- First AWS Service
- Pull-based, not pushed based (SNS is push-based)
- Messages can be kept in queue from 1 min to 14 days, default retention period is 4 days
- Gives you access to a message queue that can be used to store messages while waiting for a computer to process them.
- A queue is a temporary repository for messages that are awaiting processing
- Delay queues let you postpone the delivery of new messages to a queue for a number of seconds.
- Visibility time out:
    - Amount of time that the message is invisible in the SQS queue after a reader picks up that message.
    - Provided the job is processed before the visibility time out expires, the message will then be deleted from the queue.
    - If the job is not processed within that time, the message will become again and another reader will process it.
        - Can result in the same message being delivered twice
    - Decault value is 30 seconds
    - Maximum is 12 hours
- Messages can contain up to 256 KB of text in any format, you can go above that using S3 if you wish
- The queue acts as a buffer between the component producing and saving data, and the component receiving the data for processing
- Guarantees that your messages will be processed at least once.
- Long polling is a way to retrieve messages from SQS queues, doesn't return a response until a message arrives in the queue or the long poll times out
    - You enable long polling by setting the ReceiveMessageWaitTimeSeconds to a number > 0
- 2 types of queues:
    1. Standard
        - guarantee that a message is delivered at least once
        - Lets you have nearly unlimited number of transactions per second
        - Occassionally more than one copy of a message might be delivered out of order
        - This is the default queue type
    2. FIFO
        - Exactly one-time processing
        - Dubplicates are not introduced into the queue
        - Order in which messages are sent and received is strictly preserved
        - limited to 300 transactions/second
        - Support message groups
- Message-oriented API

## SWF
- Amazon Simple Workflow Service
- Web Service that makes it easy to coordinate work across distributed application components
- Use cases:
    - Media processing
    - Web application backends
    - Business process workflows
    - Analytics pipelines
- Task represent invocations of various processing step in an application
    - Can be performed by:
        - Executable code
        - Web Service Calls
        - Human Actions
        - Scripts
- Can combine digital environment along with manual tasks performed by human beings
- Workflow executions can last up to one year
- Task-oriented API
- Keeps track of all the tasks and events in an application (compared to SQS where you need to implement your own application-level tracking)
- Actors:
    - Workflow Starters
        - Application that can initiate a workflow
    - Deciders
        - Control the flow of activity tasks in a workflow execution
    - Activity Workers
        - Carry out the activity tasks
- Domains provide a way of scoping Amazon SWF resources within your AWS account. 
    - Represent a collection of related workflows

## SNS
- Amazon Simple Notification Service is a web service that makes it easy to set up, oeprate, and send notifications from the cloud
- Publish messages from an application and immediately deliver them to subscribers or other applications
- Supports push notifications to mobile devices
- Can also deliver notifications by SMS text message or email ro to any HTTP endpoint
- Allows to group multiple recipients uring topics
    - Topic:
        - "Access point" for allowing recipients to dynamically subscribe for identical copies of the same notification
        - Can support deliveries to multiple endpoint types
- All messages published to SNS are stored redundantly across multiple AZs
- Benefits:
    - Instantaneous, push-based delivery (no polling)

## Elastic Transcoder
- Media transcoder in the cloud
- Convert media files from their original source format in to different formats that will play on smartphones, tablets, PCs, etc.
- Provides transcoding presets for popular output formats (no need to guess which settings work best on particular devices)
- Pay based on the minutes that you transcode and the resolution at which you transcode
- Ex:  S3 bucket-->Lambda Function-->Elastic Transcoder-->S3 bucket

## API Gateway
- API Gateway is a fully managed service that makes it easy for developers to publish, maintain, monitor, and secure APIs at any scale
- Allows the creation of "front door" for applications to access data, business logic, or functionality from your backend services, such as apps running on EC2, code running on lambda, or any web application
- Typically used to communicate with lambda functions
- Features:
    - Expose HTTPs endpoints to define a RESTful API
    - Connect "serverless-ly" to services like lambda & DynamoDB
    - Send each API endpoint to a different target
    - Run efficiently with low cost
    - Scale effortlessly
    - Track and control usage by API key
    - Throttle requests to prevent attacks
    - Connect to Cloudwatch to log all requests for monitoring
    - Maintain multiple versions of your API
    - Can be throttled to prevent attacks
- Configuration:
    - Define an API (container)
    - Define resources and nested resources (URL paths)
    - For each resource:
        - Select supported HTTP methods (verbs)
        - Set Security
        - Choose target (EC2, lambda, dynamoDB, etc)
        - Set request and response transformations
    - Deploy gateway:
        - Deploy API to a stage:
            - Uses API Gateway domain by default
            - Can use custom domain
            - Supports AWS Certificate Manager:  free SSL/TLS certs
- API caching:
    - allow to reduce the number of calls made to your endpoint and also improve latency of requests to API
    - Caches responses from your endpoint for specific TTL period in seconds
    - Response is obtained from chace instead of making request to endpoint
- CORS:  Cross Origin Resource Sharing
    - Same Origin Policy
        - A web browser only permits scripts contained in a frist web page to access data in a second web page if both pages have the same origin (same domain name)
        - Prevents XSS attacks (Cross Site Scripting)
        - Enforced by web browsers
        - Ignored by tools such as Postman and curl
    - CORS is one way the server at the other end (not the client code in the browser) can relax the same origin policy
    - Mechanism that allows restricted resources (e.g fonts) on a web page to be requested from another domain outside the domain from which the first resource was served
    - CORS is enforced by the client (browser)
    - CORS in action:
        - Browser makes an HTTP Options call for a URL
        - Server returns a response that says:  "These other domains are approved to GET this URL"
        - Server can return:  "Origin policy cannot be read at the remote resource"


## Step Functions (SFN)
- AWS Step Functions is a fully managed service that makes it easy to coordinate the components of distributed applications and microservices using visual workflows.

## Kinesis
- Streaming data is data that is generated continuosly by thousands of data sources, which typically send in the data records simultaneously and in small sizes (order of kilobytes)
    - Examples of streaming data:
        - Purchases from online stores
        - Stock prices
        - Game data (as the gamer plays)
        - Social network data
        - Geospatial Data (ex:  uber)
        - iOT sensor data
- Kinesis is a platform on AWS to send your streaming data to, making it easy to load and analyze streaming data, and also providing the ability for you to build your own custom applications for your business needs.
- 3 different types of kinesis:
    1. Kinesis Streams
        - Data Producers stream data to kinesis
        - Kinesis streams is a place to put all that data, default retention is 24 hours but can go up to 7 days
        - Data is contained in shards
        - Data consumers (EC2 instances) can go an analyze that data in shards
        - After data has been processed it can be stored in different places:    DynamoDB, S3, EMR, Redshift
        - Shards:
            - 5 transactions/sec for reads and up to a max total data read of 2MB/sec
            - Up to 1000 records/sec for writes and up to max total data write rate of 1MB/sec
            - Total capacity of the stream is the sum of the capacities of its shards
    2. Kinesis Firehose
        - No persistent storage
        - You need to do something with that data as soon as it comes in, normally using lambda functions and outputting somewhere safe (S3-->Redshift, or ElasticSearch Cluster)
    3. Kinesis Analytics
        - Works with Kinesis Streams and Kinesis Firehose
        - It can analyze the data on the fly
        - Can store data either on S3, Redshift or Elastic Search Cluster

## Cognito
    - Web Identity Federation
        - Lets you give your users access to AWS resources after they have successfully authenticated with a web-based identity provider like Amazon, Facebook, Google
        - Following successful authentication the user receives an authentication token from the WEB ID provider, which they can trade for temporary AWS security credentials
    - Features:
        - Sign-up and sign-in to your apps
        - Access for guest users
        - Acts as Identity Broker between your application and WebID providers, no need to write additional code
        - Synchronizes user data for multiple devices
        - Recommended for all mobile applications using AWS services
        - Cognito Synchronization
            - Tracks the association between user identity and the various different devices they sign-in from
            - Uses push synchronization to push updates and synchronize data across multiple devices
                - Uses SNS to send a notification to all devices associated with a given user identity whenever data stored in the cloud changes
            - Scenarios:  User updates email address, changes pwd, etc.
    - User Pools
        - User directories used to manage sign-up and sign-in functionality for mobile and web applications
        - Users can sign-in directly to the User Pool, or using Facebook, Amazon. or Google.
        - Cognito acts as an Identity Provicer broker between the identity provider and AWS
        - Successful authentication generates a JSON Web Token (JWTs)
        - User Pools (by themselves) don’t deal with permissions at the IAM-level. Rather, they provide information like group membership and the user’s ID to your app, so you can deal with authorization yourself.
    - Identity Pools (aka Federated Identities)
        - Enable to provide temporary AWS credentials to access AWS services like S3 or DynamoDB
        - Are all about the authorization of access to AWS resources



