---
layout: doc
subpage: Documentation
title: Java Tutorial
description: Getting started using the NLP-NEWT Java SDK.
---

### Requirements

- Python 3.6 or later
- Oracle or OpenJDK JDK 8

### Installing nlpnewt

The Java SDK for NLP-NEWT does not need to be installed, it is distributed as
a Jar. Eventually a distribution will be made public on Maven Central for
inclusion in Maven and Gradle projects.

We will need to install the NLP-NEWT python SDK, which includes the events
service necessary to run our processor.

```bash
pip install nlpnewt-{{ site.python_version }}.tar.gz
```


### Creating a processor

We will be creating a document processor which simply writes a hello world
label to a document.

To start, create a file named ``Hello.java``, this file will contain our
processor:

```java
import edu.umn.nlpnewt.*;

@Processor("hello")
public class Hello extends DocumentProcessorBase {
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

Note use of the try-with-resources statement: ``try () {}``. This block will
cause the labeler to upload any added labels to the events service when exited.

### Compiling the processor

To compile the processor, run:

```bash
javac -cp nlpnewt-all-{{ site.version }}.jar Hello.java
```

### Running the processor

First, we will need to launch the python events service.

```bash
python -m nlpnewt events -a localhost -p 9090
```

Now we can deploy our Java processor. In another terminal run:

```bash
java -cp .:nlpnewt-all-{{ site.version }}.jar edu.umn.nlpnewt.Newt processor -p 9092 -e localhost:9090 Hello
```
