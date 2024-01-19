# cloud-workflow-mwe

Minimal working example for cloud workflow using CAVE, Amazon SQS, Google Cloud Storage, etc.

## Diagram

```mermaid
flowchart LR
    queuer["queuer\n(Python file)"]

    queue["task queue\n(Amazon SQS)"]
    click queue "https://aws.amazon.com/sqs/" "Amazon SQS"

    deadletter["dead letter queue\n(Amazon SQS)"]
    click deadletter "https://en.wikipedia.org/wiki/Dead_letter_queue" "Dead letter queue"

    worker1["worker\n(Dockerized python)"]
    worker2["worker\n(Dockerized python)"]
    worker3["worker\n(Dockerized python)"]
    worker4["worker\n(Dockerized python)"]

    storage["cloud storage\n(Google Cloud Storage)"]
    click storage "https://cloud.google.com/storage" "Google Cloud Storage"

    analysis["analysis\n(Python file)"]

    subgraph compute ["compute cluster (Google Kubernetes Engine)"]
        direction LR
        subgraph node1[node 1]
            worker1
            worker2
        end
        subgraph node2[node 2]
            worker3
            worker4
        end
    end

    queuer -- "puts jobs" --> queue
    queue -- "puts unrunnable jobs" --> deadletter
    deadletter -- "debug" --> queuer
    queue -- "pulls jobs" --> worker1
    queue -- "pulls jobs" --> worker2
    queue -- "pulls jobs" --> worker3
    queue -- "pulls jobs" --> worker4
    worker1 -- "puts data" --> storage
    worker2 -- "puts data" --> storage
    worker3 -- "puts data" --> storage
    worker4 -- "puts data" --> storage
    storage -- "pulls data" --> analysis

```

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

Currently, authentication is done using service accounts. You'll need an administrator to give you
the public/private keys for a service account, likely with read/write access to a bucket.

## Amazon SQS

Make sure that your `aws-secret.json` is set. This should look something like this:

```
{
    "AWS_ACCESS_KEY_ID": "<your access key ID>",
    "AWS_SECRET_ACCESS_KEY": "<your secret access key ID>",
    "AWS_DEFAULT_REGION": "us-west-1"
}
```

Assuming you have access to SQS, you can find this information from the AWS console by
clicking on the account name in the top-right corner -> "Security credentials" -> "Access keys" ->
"Create access key". You will be able to see the access key and secret access key on the page that pops up.

## Docker

## Google Kubernetes Engine
