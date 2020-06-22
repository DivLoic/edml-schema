# EDML Schema
###### Schemas from the [event driven ml](https://blog.loicmdivad.com/talks/event-driven-machine-learning-xebicon19/) experiment

![EDML Schema Workflow Generation](https://github.com/DivLoic/edml-schema/workflows/EDML%20Schema%20Workflow%20Generation/badge.svg)

## About EDML
Event-Driven Machine Learning is an experiment ran by 
[@DivLoic](https://github.com/DivLoic) and [@Giulbia](https://github.com/giulbia).
The idea is to bring together all the components to build a system serving prediction in realtime.
From exploring the data with interactive tools like [Ai Platform](https://cloud.google.com/ai-platform/) 
to deploying streaming application in the cloud with [Confluent Cloud](https://www.confluent.io/confluent-cloud/)
the EDML project discuss the important points leading to model serving automation.

## Why?

## How?
The module uses the [avrohugger](https://github.com/julianpeeters/avrohugger) maven plugin.

```xml
<plugin>
    <groupId>at.makubi.maven.plugin</groupId>
    <artifactId>avrohugger-maven-plugin</artifactId>
    <version>${avrohugger-maven-plugin.version}</version>
</plugin>
```

The Github Action trigger the goal `generate`:

```bash
./mvnw -B avrohugger:generate-scala-sources
```

Which creates the scala classes (under: `target/generated-sources`) that get shipped with the JAR. Example:

```scala
case class KeyClass(var uuid: String) extends org.apache.avro.specific.SpecificRecordBase {
  def this() = this("")
  def get(field$: Int): AnyRef = {
    (field$: @switch) match {
      case 0 => {
        uuid
      }.asInstanceOf[AnyRef]
      case _ => new org.apache.avro.AvroRuntimeException("Bad index")
    }
  }
  def put(field$: Int, value: Any): Unit = {
    (field$: @switch) match {
      case 0 => this.uuid = {
        value.toString
      }.asInstanceOf[String]
      case _ => new org.apache.avro.AvroRuntimeException("Bad index")
    }
    ()
  }
  def getSchema: org.apache.avro.Schema = KeyClass.SCHEMA$
}
```

## See also
- [EDMl at Xebicon'19](https://youtu.be/g646cjDvg84)
- [EDML: Event-driven ml slides](https://speakerdeck.com/loicdivad/event-driven-machine-learning)
- [EDMl on Confluent Webinar](https://www.confluent.io/online-talks/event-driven-machine-learning-avec-publicis-sapient/)