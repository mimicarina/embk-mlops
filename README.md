# MLOps Lab


In this lab, we will create and experiment an end-to-end ML Pipeline. 
This ML pipeline is supported by an automated infrastructure for training, testing, deploying and integrating ML Models. 

Furthermore, the model can be deployed in two different environments: DEV environment for QA/Development endpoint and PRD environment for Production endpoint. 
In addition, we will apply some stress tests to check the ML system scalability and stability. 


---

<a name="Pre-requisites"/>

## Pre-requisites

### AWS Account

You'll need an AWS Account with access to the services above. There are resources required by this workshop that are eligible for the [AWS Free Tier](https://aws.amazon.com/free/) if your account is less than 12 months old.

**WARNING**: if your account is more than 12 months old, you may get a bill.

### Knowledge Check

You should have some basic experience with:
  - Train/test a ML model
  - Python ([scikit-learn](https://scikit-learn.org/stable/#))
  - [Jupyter Notebook](https://jupyter.org/)
  - [AWS CodePipeline](https://aws.amazon.com/codepipeline/)
  - [AWS CodeCommit](https://aws.amazon.com/codecommit/)
  - [AWS CodeBuild](https://aws.amazon.com/codebuild/)
  - [Amazon ECR](https://aws.amazon.com/ecr/)
  - [Amazon SageMaker](https://aws.amazon.com/sagemaker/)
  - [AWS CloudFormation](https://aws.amazon.com/cloudformation/)


Some experience working with the AWS console is helpful as well.

---

<a name="Architecture"/>

## The ML Pipeline Architecture

The following image gives us a high level view of the architecture.

<p align="center">
  <img src="imgs/MLOps_Train_Deploy_TestModel.jpg" alt="drawing" width="600"/>
</p>

1. An ETL process or the ML Developer, prepares a new dataset for training the model and copies it into an S3 Bucket;
2. CodePipeline listens to this S3 Bucket, calls a Lambda function for starting training for a job in Sagemaker;
3. The lambda function sends a training job request to Sagemaker;
4. When the training is finished, CodePipeline gets its status and goes to the next stage if there is no error;
5. CodePipeline calls CloudFormation to deploy a model in a Development/QA environment into Sagemaker;
6. After finishing the deployment in DEV/QA, CodePipeline awaits for a manual approval
7. An approver approves or rejects the deployment. If rejected the pipeline stops here; If approved it goes to the next stage;
8. CodePipeline calls CloudFormation to deploy a model into production. This time, the endpoint will count with an AutoScaling policy for HA and Elasticity.
9. Done.


----
## License Summary
This sample code is made available under a modified MIT license. See the LICENSE file.

