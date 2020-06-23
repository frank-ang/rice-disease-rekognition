# Rekognition Custom Images

## Data Sets:

* https://archive.ics.uci.edu/ml/datasets/Rice+Leaf+Diseases

* https://www.kaggle.com/jonathanrjpereira/rice-disease
	** BrownSpot, Healthy, Hispa, LeafBlast

* appears similar to:
  https://www.kaggle.com/minhhuy2810/rice-diseases-image-dataset


# API test.

## start API:

aws rekognition start-project-version \
  --project-version-arn "arn:aws:rekognition:us-east-1:450428438179:project/rice-iad/version/rice-iad.2020-02-02T15.54.48/1580630090445" \
  --min-inference-units 1 \
  --endpoint-url https://rekognition.us-east-1.amazonaws.com \
  --region us-east-1

## detect API:

aws rekognition detect-custom-labels   --project-version-arn "arn:aws:rekognition:us-east-1:450428438179:project/rice-iad/version/rice-iad.2020-02-02T15.54.48/1580630090445"   --image '{"S3Object": {"Bucket": "sandbox00-aiml-iad","Name": "unknown/LeafBlast-01.jpg"}}'   --endpoint-url https://rekognition.us-east-1.amazonaws.com   --region us-east-1

## Stop API

aws rekognition stop-project-version \
  --project-version-arn "arn:aws:rekognition:us-east-1:450428438179:project/rice-iad/version/rice-iad.2020-02-02T15.54.48/1580630090445" \
  --endpoint-url https://rekognition.us-east-1.amazonaws.com \
  --region us-east-1