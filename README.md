# Rice disease image detection with Amazon Rekognition Custom Labels.

At the time of this writing, Amazon Rekognition Custom Labels may be available in select AWS Regions only. In this demo, I use us-east-1 (Virginia).

## Data Sets:

Load training data set into S3.

* https://archive.ics.uci.edu/ml/datasets/Rice+Leaf+Diseases

* https://www.kaggle.com/jonathanrjpereira/rice-disease
	** BrownSpot, Healthy, Hispa, LeafBlast

* appears similar to:
  https://www.kaggle.com/minhhuy2810/rice-diseases-image-dataset

## Train a model.

See Amazon Rekognition Custom Labels docs: https://docs.aws.amazon.com/rekognition/latest/customlabels-dg/gs-introduction.html

## Test the model

### Set some parameters.
```
export AWS_DEFAULT_REGION=us-east-1

PROJECT_ARN_LIST=`aws rekognition describe-projects --output text --query "ProjectDescriptions[].ProjectArn"`

echo "Displaying project versions"
for PROJECT_ARN in $PROJECT_ARN_LIST; do
  aws rekognition describe-project-versions --project-arn $PROJECT_ARN \
    --output text --query "ProjectVersionDescriptions[].[ProjectVersionArn,Status]"
done
```

Set the project version that you need. E.g.
```
PROJECT_VERSION=something_ARN
```

### Start model

```
aws rekognition start-project-version \
  --project-version-arn $PROJECT_VERSION \
  --min-inference-units 1 
```
This should take <10 minutes to start. 

### Detect custom labels on an image:

Here, we look for rice disease in a test image.
```
TEST_BUCKET=sandbox00-aiml-iad
TEST_IMAGE=unknown/LeafBlast-01.jpg
aws rekognition detect-custom-labels  --project-version-arn $PROJECT_VERSION \
  --image '{"S3Object": {"Bucket": "'$TEST_BUCKET'","Name": "'$TEST_IMAGE'"}}'
```
Sample result:
```
{
    "CustomLabels": [
        {
            "Name": "LeafBlast",
            "Confidence": 99.9990005493164
        }
    ]
}
```

### Stop model

> **WARNING: ⚠️ Remember to stop the model after testing**,  to be frugal with AWS costs. 
>
> Inference currently costs US$4 per hour in US Regions. See https://aws.amazon.com/rekognition/pricing/

```
aws rekognition stop-project-version \
  --project-version-arn $PROJECT_VERSION
```