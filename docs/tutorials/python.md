---
layout: doc
subpage: Documentation
title: Python Tutorial
description: Getting started using the NLP-NEWT Python SDK.
---

## Requirements


- Python 3.5 or later

## Installing nlpnewt

To install nlpnewt run the following:

```bash
pip install nlpnewt-{{site.python_version}}.tar.gz
```

This is an sdist for now, but will be on PyPi eventually.

## Creating a processor


We will be creating a document processor which simply writes a hello world label to a document.

To start, create a file named ``hello.py``, this file will contain our processor:

```python
import nlpnewt
from nlpnewt.processing import DocumentProcessor

@nlpnewt.processor('hello')
class HelloProcessor(DocumentProcessor):
   def process_document(self, document, params):
       with document.get_labeler('hello') as hello_labeler:
           text = document.text

           hello_labeler(0, len(text), response='Hello ' + text + '!')

if __name__ == '__main__':
   nlpnewt.run_processor(HelloProcessor())
```

This file receives a request to process a document, and then labels that documents text with
a hello response.

## Running the processor

To host the processor, run the following commands in terminal windows (they run in the foreground)

```bash
python -m nlpnewt events --address localhost --port 9090

python hello.py --address localhost --port 9091 --events localhost:9090
```

To perform processing, create another file ``pipeline.py``:

```python
from nlpnewt import Document, Event, EventsClient, Pipeline, RemoteProcessor

with EventsClient(address='localhost:9090') as client, \
     Pipeline(
         RemoteProcessor(processor_id='hello', address='localhost:9091')
     ) as pipeline:
  with Event(event_id='1', client=client) as event:
    document = Document(document_name='name', text='YOUR NAME')
    event.add_document(document)
    pipeline.run(document)
    index = document.get_label_index('hello')
    for label in index:
      print(label.response)
```


To run this pipeline type

```bash
python pipeline.py
```

## Interacting with the Events Service

When you deploy the processor above, the NLP-NEWT framework wraps that processor
in a gRPC service and server. When it receives a request, it uses the request
parameters to instantiate the ``Document`` object that gets passed to your
processor. This ``Document`` object is your interface to the events service.

For more information about the methods that are available, see the
[API Documentation](https://nlpie.github.io/newt-python-api/nlpnewt.html#nlpnewt.events.Document).
