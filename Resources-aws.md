* API https://github.com/seratch/AWScala
* Reference Architectures http://aws.amazon.com/architecture/?nc1=f_cc
* Web Application Hosting in the AWS Cloud http://media.amazonwebservices.com/AWS_Web_Hosting_Best_Practices.pdf
* Instructional Videos and Labs http://aws.amazon.com/training/intro_series/

SQS
* Building Scalable, Reliable Amazon EC2 Applications with Amazon SQS http://sqs-public-images.s3.amazonaws.com/Building_Scalabale_EC2_applications_with_SQS2.pdf
* http://aws.amazon.com/sqs/developer-resources/

S3
* Receive SQS/SNS/Lambda notification when new object is created http://aws.amazon.com/blogs/aws/s3-event-notification/

EC2
* Compare cost of different EC2 configurations http://www.ec2instances.info/

DynamoDB
* Powering Gaming Applications with Amazon DynamoDB http://blogs.aws.amazon.com/bigdata/post/Tx27Y7HOQ4V6393/Powering-Gaming-Applications-with-Amazon-DynamoDB
  * support thousands of players for the price of a daily cup of coffee
  * fake_dynamo https://github.com/ananthakumaran/fake_dynamo
  * auto-incrementing primary keys become a bottleneck. Use UUID
  * Dynamic DynamoDB manages the process of scaling the provisioned throughput for your DynamoDB tables
    * http://aws.amazon.com/blogs/aws/auto-scale-dynamodb-with-dynamic-dynamodb/

* Building Talko with DynamoDB
  * https://medium.com/talko-team-talk-share-do/building-talko-with-dynamodb-dcae34b94a4e
  * Short timeout 1 second for the first try comma 5 seconds for the second one
  * Log on retry do we also need a metric 4 that
  * Avoid large items. Large items can cause your costs to increase significantly
  * Ddb request should be idempotent due to timeout and retry. Watch for conditional writes, e.g. write succeeds, client times out, client retries and gets an error.
  * Test behavior at throughput limit
  * Reads are a lot cheaper than writes. If the item most likely exists, do read first as this will allow you to save write unit.
