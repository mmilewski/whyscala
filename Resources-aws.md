* API https://github.com/seratch/AWScala
* Reference Architectures http://aws.amazon.com/architecture/?nc1=f_cc
* Web Application Hosting in the AWS Cloud http://media.amazonwebservices.com/AWS_Web_Hosting_Best_Practices.pdf
* Instructional Videos and Labs http://aws.amazon.com/training/intro_series/

SQS
* Building Scalable, Reliable Amazon EC2 Applications with Amazon SQS http://sqs-public-images.s3.amazonaws.com/Building_Scalabale_EC2_applications_with_SQS2.pdf
* http://aws.amazon.com/sqs/developer-resources/

EC2
* Compare cost of different EC2 configurations http://www.ec2instances.info/

DynamoDB
* Powering Gaming Applications with Amazon DynamoDB http://blogs.aws.amazon.com/bigdata/post/Tx27Y7HOQ4V6393/Powering-Gaming-Applications-with-Amazon-DynamoDB
  * support thousands of players for the price of a daily cup of coffee
  * fake_dynamo https://github.com/ananthakumaran/fake_dynamo
  * auto-incrementing primary keys become a bottleneck. Use UUID
  * Dynamic DynamoDB manages the process of scaling the provisioned throughput for your DynamoDB tables
    * http://aws.amazon.com/blogs/aws/auto-scale-dynamodb-with-dynamic-dynamodb/
