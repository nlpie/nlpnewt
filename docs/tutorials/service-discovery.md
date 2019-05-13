---
layout: doc
subpage: Documentation
title: Service Discovery Tutorial
description: >
  Using Consul service discovery to automatically register and discover
  addresses and ports for NEWT services.
---

## Requirements

- [Consul](https://consul.io)

## Starting Services using service discovery

Once consul is running, services can be started and registered to consul using
the following commands:

```bash
python -m nlpnewt events --register

python -m nlpnewt processor -m {processor_module} -n {processor_name} --register

java -cp .:nlpnewt-all-{{ site.version }}.jar edu.umn.nlpnewt.Newt processor --register {processor_class}
```

## Running pipelines using service discovery

Taken from the
[Python Tutorial]({{'/docs/tutorials/python.html' | relative_url}}), we can
run this pipeline using service discovery by removing the addresses:

```python
import nlpnewt

with nlpnewt.Events() as events, nlpnewt.Pipeline() as pipeline:
  pipeline.add_processor('hello')
  with events.open_event('1') as event:
    document = event.add_document('name', 'YOUR NAME')
    pipeline.process_document(document)
    index = document.get_label_index('hello')
    for label in index:
      print(label.response)
```
