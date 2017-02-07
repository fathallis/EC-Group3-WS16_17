# EC-Group3-WS16_17

# Introduction / Challenge

In this repository you can find the result for the Assignement of the course Enterprise Computing at TU Berlin. As a Group of four our task was to rebuild a AWS serverless reference architecture for Real-time Stream Processing on either IBM Bluemix or Google Cloud Platform. Because of the work with IBM Bluemix during the exercises of the Enterprise Computing course we decided to implement our solution with it. Also there were not enough services provided by the Google Cloud Platform to fully reimplement the reference architecture.

# AWS Serverless Reference Architecture: Real-time Stream Processing

![alt tag](https://github.com/fathallis/EC-Group3-WS16_17/blob/master/aws-serverless-streamprocessing.png)

All informations can be found on this website: http://www.allthingsdistributed.com/2016/06/aws-lambda-serverless-reference-architectures.html

... and on the this Git repository: https://github.com/awslabs/lambda-refarch-streamprocessing

# Result: Real-time Stream Processing on IBM Bluemix

The following picture shows a serverless architecture for Real-time Stream Procecessing built on IBM Bluemix: 

![Screenshot](https://cloud.githubusercontent.com/assets/19613306/22619178/44d8ca3c-eaef-11e6-92ab-2778f3623527.png)

Similar to AWS Kinesis we used IBM Message Hub to create a stream or queue, where we can push twitter messages into. In order to do so we needed to implement an application (Java), which uses the twitter API to filter messages for specific content (hashtags). The application then pushs the filteres messages with several other informations like time and username into the Message Hub. 

As soon as a message/ messages arrive/s into the Message Hub, a OpenWhisk (OW) function (implemented in node.js) is triggered. OW does basically the same as AWS Lambda. By triggering the OW function, Message Hub automatically passes the message/s (JSON) in the queue to the OW fucntion. The OW function has then the task to parse the delivered messages and store them into CloudantDB, a NoSQL Storage service of IBM Bluemix. The messages are concatenated to the existing table of messages in  CoudantDB.

By connecting CloudantDB with DashDB for Analytics, activities and transactions in CloudantDB can be tracked and monitored. DashDB, which is a SQL Cloud Storage Service of the IBM Bluemix Platform, pulls data from the CloudantDB and creates a warehouse, where the data can be analysed.

It was not possible to use the Oject Storage Service of IBM Bluemix with a trial student account. Therefore something equal to AWS S3 could not be implemented. 

In this repository you can find the implemented Java application for filtering Twitter messages for certain content and pushing those to Message Hub (folder: "twitter-to-message-hub"). Also you can find the implemented OpenWhisk function, which receives the messages from Message Hub and Stores them into Cloudant DB (folder: "OpenWhisk"). There you will find more detailed information about the application/ function.

