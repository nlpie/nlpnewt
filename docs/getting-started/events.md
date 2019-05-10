---
layout: doc
title: Events Model
subpage: Documentation
description: How data is stored during processing.
---
## Events Data Model
### Event

The top level data object is an ``Event``. An event's primary function is to
store documents, it functions as a dictionary / mapping of names to documents.
Events are stored on a service called the "events service" which stores and
provides the data associated with an event for the lifetime of the event. The
event is primarily identified by a unique ``event_id``.

When processors are called, they are sent an ``event_id`` and that identifier is
used to access access the data the processor needs from the events service.

### Document
