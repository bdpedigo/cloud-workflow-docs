# cloud-workflow-mwe

Minimal working example for cloud workflow using CAVE, Amazon SQS, Google Cloud Storage, etc.

## CAVEclient
From a fresh virtual environment,
```
pip install caveclient
```
Once `caveclient` is installed, make sure authentication is set up.
```
import caveclient as cc
client = cc.CAVEclient()
auth = client.auth
auth.get_new_token()
```
Follow the instructions that pop up. This will involve something like 
```
auth.save_token(token=<your new token>)
```

## Google Cloud Storage 

## Amazon SQS

## Docker 

## Google Kubernetes Engine
