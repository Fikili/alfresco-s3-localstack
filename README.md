# alfresco-s3-localstack
## Hackathon
This project was implemented as part of Alfresco virtual hackathon 2021.
## Motivation
Nowadays applications are deployed more and more to a cloud. Since Alfresco first choice for the cloud provider is AWS, this project will focus on running "local" AWS environment using [LocalStack](https://github.com/localstack/localstack/blob/master/README.md).

Currently, LocalStack allows mocked AWS S3 storage, the project will use especially this AWS service. AWS RDS (managed service for database) is currently available in LocalStack **Pro**, so it will not be used.

Using LocalStack all developers can test a source code it doesn't require AWS account. Another advantage is possible integration testing with mocked AWS service(s). 
## Steps to be done
- SDK 4.2 with Alfresco Community 7
- Add common dependencies:
  - [OOTBee Support Tools](https://github.com/OrderOfTheBee/ootbee-support-tools)
  - [Javascript Console](https://github.com/share-extras/js-console)
- Adding community project for integration with AWS S3:
  - [Alfresco S3 Adapter](https://github.com/Redpill-Linpro/alfresco-s3-adapter)
  - Configure AWS credentials for testing -> If OK, continue with next step
- Configure LocalStack to work with mocked S3 storage  
- "For fun part": Add SDK5 (out-of-process) project that will simulate behaviours and move content to different locations
## Things to remember
**This is PoC project. Using that in PROD is on your own risk.**
## Hackathon result in one sentence
- It's possible to use [Alfresco S3 Adapter](https://github.com/Redpill-Linpro/alfresco-s3-adapter) with [LocalStack](https://github.com/localstack/localstack/blob/master/README.md)
## How to run the project in more detailed description 
- Build alfresco-s3-adapter project - It is used as library for communication with AWS 
  - `$ git clone https://github.com/Redpill-Linpro/alfresco-s3-adapter.git`
  - `$ mvn clean install -DskipTests`
    - Or use [Maven artifactory](https://maven.redpill-linpro.com/nexus/content/repositories/public/org/redpill-linpro/alfresco/s3/) provided by Redpill-Linpro
- Install the latest versions of Docker + Docker Compose + AWS CLI 2
- Clone LocalStack project and run required services
  - `$ git clone https://github.com/localstack/localstack.git`
  - Update SERVICES to use just S3 + set UI port to different than 8080 + TMPDIR
    - `$ TMPDIR=/tmp PORT_WEB_UI=9999 SERVICES=s3 docker-compose up -d`
    - If everything is OK, you should see `localstack_main | Ready.`
- Create profile for AWS localstack user
  - `$ aws configure --profile localstack`
- Create S3 bucket in LocalStack
  - `$ aws s3api --endpoint-url http://localhost:4566 create-bucket --bucket my-bucket --region eu-central-1 --profile localstack`
  - As result, you should see `{"Location": "/my-bucket"}`
- Configure Alfresco to use LocalStack in alfresco-global.properties
  - aws.accessKey=123 # AFAIK, it is not used but usually it should correspond to AWS credentials 
  - aws.secretKey=123456 # Same as above
  - aws.regionName= # If empty, default is US-EAST-1
  - aws.s3.bucketName=my-bucket # Name corresponds to bucket name that was created in LocalStack
  - aws.s3.endpoint=http://XXX.XXX.XXX.XXX:4566 # Change XXX by proper IP address where LocalStack is running
  - aws.s3.rootDirectory=/alfresco/contentstore # Root folder for Alfresco content in S3 bucket. If missing, it is created automatically.
- Verification that content is in S3 bucket provided by LocalStack
  - Start Alfresco SDK and see that files are stored in LocalStack
    - `$ aws s3 --endpoint-url http://XXX.XXX.XXX.XXX:4566 ls --recursive s3://my-bucket --profile localstack`
    - Expected result:
    ```
    2021-06-16 13:17:47      20964 alfresco/contentstore/2021/6/16/17/17/00369794-aef6-4203-b40b-e4d047b0c9ae.bin
    2021-06-16 13:17:47      74240 alfresco/contentstore/2021/6/16/17/17/003f93ec-2c2a-43ef-876e-fe044e52c513.bin
    ...
    ```