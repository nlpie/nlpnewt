---
layout: doc
subpage: Documentation
title: Service Discovery Tutorial
description: >
  Using Consul service discovery to automatically register and discover
  addresses and ports for NEWT services.
---

## Requirements

- Python 3.5
- [Consul](https://consul.io)
- Java JDK 8 (optional)

## Starting consul

To start a local dev instance of consul, run the following:

```bash
consul agent -dev -ui
```

## Starting Services using service discovery

Once consul is running, services can be started and registered to consul using
the following commands:

```bash
python -m nlpnewt events --register

python -m [processor_module] --register

java -cp .:nlpnewt-all-{{ site.version }}.jar [ProcessorClass] --register
```

## Running pipelines using service discovery

Taken from the
[Python Tutorial]({{'/docs/tutorials/python.html' | relative_url}}), we can
run this pipeline using service discovery by removing the addresses:

```python
from nlpnewt import EventsClient, Pipeline, RemoteProcessor


with EventsClient() as client, \
     Pipeline(
         RemoteProcessor(processor_id='hello')
     ) as pipeline:
  with Event(event_id='1', client=client) as event:
    document = event.add_document(document_name='name', text='YOUR NAME')
    pipeline.run(document)
    index = document.get_label_index('hello')
    for label in index:
      print(label.response)
```
