---
layout: doc
title: Processing
subpage: Documentation
description: How processing documents works using NLP-NEWT.
---

## Introduction

In NLP-NEWT there are two types of processing components:

- EventProcessor
- DocumentProcessor

Event processors perform processing on ``Event`` objects, uploading the results
to the event service using Labelers.

Document processors perform processing on a specific ``Document`` object on an
``Event``, which is specified through a ``document_name`` processing parameter.

Either a ``Pipeline`` object or the API gateway can be used to trigger
processing. Newt handles retrieving a reference to the ``Event`` or ``Document``
from an identifier, and passes that object to the processor.

## Details

The NLP-NEWT framework provides functionality that takes processors classes,
instantiates them, and then wraps them in a gRPC service that respond to
specific processing endpoints. After this deployment, when the processor is
called, NLP-NEWT adapts the processing arguments from the gRPC request object
into objects that the processor can use.
