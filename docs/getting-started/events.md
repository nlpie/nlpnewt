---
layout: doc
title: Events and Documents Data Model
subpage: Documentation
description: >
  How data is stored during processing. The following are classes and data
  structures that are provided by the SDK to interact with the Events Service.
---
## Event

The top level data object is an ``Event``. An event's primary function is to
store documents, it functions as a dictionary / mapping of names to documents.
Events are stored on a service called the "events service" which stores and
provides the data associated with an event for the lifetime of the event. The
event is primarily identified by a unique ``event_id``.

When processors are called, they are sent an ``event_id`` and that identifier is
used to access access the data the processor needs from the events service.

## Document

A ``Document`` is text and a mapping of names to ``LabelIndex`` objects. Label
indices are added using a ``Labeler`` and once retrieved.

For concurrency reasons, Documents can only be augmented, the text is
immutable after construction, and the label indices are immutable after being
added, they cannot be deleted or modified.

## LabelIndex

A ``LabelIndex`` is a set of meaningful locations in text and associated
properties. It provides functionality for filtering and navigation of the Labels
it holds, which are stored as an array sorted by the label's location in text.

## Labeler

The labeler is an object or function that is used to create a new label index
on the document.

### TODO: Add illustrative examples

## Label

A label is span of text that the system has assigned some kind of importance or
meaning. Examples could be sentences, part of speech tags, named entities,
sections, identified concepts, or higher level semantic structures.

### TODO: Add illustrative examples
