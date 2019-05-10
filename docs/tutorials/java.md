---
layout: doc
subpage: Documentation
title: Java Tutorial
description: Getting started using the NLP-NEWT Java SDK.
---

### Requirements

- Oracle or OpenJDK Java JDK 8

### Installing nlpnewt

The Java SDK for NLP-NEWT does not need to be installed, it is distributed as
a Jar.

### Creating a processor

We will be creating a document processor which simply writes a hello world label to a document.

To start, create a file named ``Hello.java``, this file will contain our processor:

```java
import edu.umn.nlpnewt.*;

@Processor("hello")
public class HelloProcessor extends DocumentProcessorBase {
  @Override
  protected void process(Document document, JsonObject params, JsonObjectBuilder result) {
    try (Labeler<GenericLabel> labeler = document.getLabeler("hello")) {
      String text = document.getText();
      labeler.add(
        GenericLabel.newBuilder(0, text.length())
          .setProperty("response", "Hello" + text + "!")
          .build()
      );
    }
  }
}
```
