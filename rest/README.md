# AdaptNLP-Rest API

## Getting Started 

#### Docker Hub
The docker image of AdaptNLP is built with the `achangnovetta/adaptnlp:latest` image.

The docker image of AdaptNLP's rest services is hosted on Docker Hub as well [here](https://hub.docker.com/r/achangnovetta/adaptnlp-rest).

This image can be pulled by running `docker pull achangnovetta/adaptnlp-rest:latest`

## Build and Run
To manually build and run the rest services by running one of the following methods in this directory:

#### 1. Docker Build Env Arg Entries
Specify the pretrained models you want to use for the endpoints.  These can be Transformers pre-trained models, Flair's pre-trained models,
or your own custom trained models with a path pointing to the model.  (The model must be in this directory)
```
docker build -t adaptnlp-rest:latest --build-arg TOKEN_TAGGING_MODE=ner \
                                     --build-arg TOKEN_TAGGING_MODEL=ner-ontonotes-fast \
                                     --build-arg SEQUENCE_CLASSIFICATION_MODEL=en-sentiment \
                                     --build-arg QUESTION_ANSWERING_MODEL=distilbert-base-uncased-distilled-squad .
docker run -itp 5000:5000 adaptnlp-rest:latest bash
```
To run with GPUs if you have nvidia-docker installed with with compatible NVIDIA drivers
```
docker run -itp 5000:5000 --gpus all adaptnlp-rest:latest bash
```

**Token Tagger**
```
docker build -t token-classification:latest --build-arg TOKEN_TAGGING_MODE=ner \
                                     --build-arg TOKEN_TAGGING_MODEL=ner-ontonotes-fast .
docker run -itp 5000:5000 adaptnlp-rest:latest bash
```
To run with GPUs if you have nvidia-docker installed with with compatible NVIDIA drivers
```
docker run -itp 5000:5000 --gpus all token-classification:latest bash
```

**Sequence Classifier**
```
docker build -t sequence-classification:latest --build-arg SEQUENCE_CLASSIFICATION_MODEL=nlptown/bert-base-multilingual-uncased-sentiment .
docker run -itp 5000:5000 sequence-classification:latest bash
```
To run with GPUs if you have nvidia-docker installed with with compatible NVIDIA drivers
```
docker run -itp 5000:5000 --gpus all sequence-classification:latest bash
```

**Question Answering**
```
docker build -t question-answering:latest --build-arg QUESTION_ANSWERING_MODEL=distilbert-base-uncased-distilled-squad .
docker run -itp 5000:5000 question-answering:latest bash
```
To run with GPUs if you have nvidia-docker installed with with compatible NVIDIA drivers
```
docker run -itp 5000:5000 --gpus all question-answering:latest bash
```

#### 2. Docker Run Env Arg Entries
Sometimes you may wont to specify the models as environment variables in docker post-build for convience or other reasons. To do so use the below commands to deploy any of the above NLP task services. The example below runs the token classification service.
```
docker build -t token-classification:latest .
docker run -itp 5000:5000 -e TOKEN_TAGGING_MODE='ner' \
                          -e TOKEN_TAGGING_MODEL='ner-ontonotes-fast' \
                          token-classification:latest \
                          bash
```
To run with GPUs if you have nvidia-docker installed with with compatible NVIDIA drivers
```
docker run -itp 5000:5000 --gpus all -e TOKEN_TAGGING_MODE='ner' \
                                     -e TOKEN_TAGGING_MODEL='ner-ontonotes-fast' \
                                     token-classification:latest \
                                     bash
```                                                           

#### Manual
If you just want to run the rest services locally in an environment that has AdaptNLP installed, you can 
run the following in whichever NLP task directory youw ould like.

```
pip install -r requirements
export TOKEN_TAGGING_MODE=ner
export TOKEN_TAGGING_MODEL=ner-ontonotes-fast
export SEQUENCE_CLASSIFICATION_MODEL=en-sentiment
export QUESTION_ANSWERING_MODEL=distilbert-base-uncased-distilled-squad
uvicorn app.main:app --host 0.0.0.0 --port 5000

```

## SwaggerUI

Access SwaggerUI console by going to `localhost:5000/docs` after deploying

![Swagger Example](https://raw.githubusercontent.com/novetta/adaptnlp/master/docs/img/fastapi-docs.png)

