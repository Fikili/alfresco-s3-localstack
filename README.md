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