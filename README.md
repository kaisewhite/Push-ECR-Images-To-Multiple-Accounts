# How to build/pull/push ECR images using different accounts

Install [amazon-ecr-credential-helper](https://github.com/awslabs/amazon-ecr-credential-helper)

1. Generate Auth Credentials

```
aws ecr get-login-password --region ${REGION} --profile ${PROFILE} | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com
```

2. Build the image

```
docker build -f ./Dockerfile --build-arg ${ARGUMENT} -t ${ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/${REPOSITORYNAME}:${IMAGETAG} .
```

3. Push the image to ECR

```
   AWS_PROFILE=${PROFILE} docker push ${ACCOUNT_ID}.dkr.ecr.us-east-1.amazonaws.com/${REPOSITORYNAME}:${IMAGETAG}
```

Sample ~ ./docker/config.json

```
{
  "experimental" : "enabled",
  "HttpHeaders" : {
    "User-Agent" : "Docker-Client/19.03.13-beta2 (darwin)"
  },
  "auths" : {
    "xxxx.dkr.ecr.us-east-1.amazonaws.com" : {
    },
    "xxxx2.dkr.ecr.us-east-2.amazonaws.com" : {
    },
    "https://index.docker.io/v1/" : {
    }
  },
  "stackOrchestrator" : "kubernetes",
  "credsStore" : "desktop",
  "credHelpers": {
    "xxxx.dkr.ecr.us-east-1.amazonaws.com": "ecr-login",
    "xxxx2.dkr.ecr.us-east-2.amazonaws.com": "ecr-login",
    "https://index.docker.io/v1/": "desktop",

  }
}
```
