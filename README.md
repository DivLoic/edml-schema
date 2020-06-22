# EDML Schema
###### Schemas from the [event driven ml](https://blog.loicmdivad.com/talks/event-driven-machine-learning-xebicon19/) experiment

![EDML Schema Workflow Generation](https://github.com/DivLoic/edml-schema/workflows/EDML%20Schema%20Workflow%20Generation/badge.svg)

## About EDML
[Event-Driven Machine Learning](https://github.com/DivLoic/event-driven-ml) is an experiment ran by 
[@DivLoic](https://github.com/DivLoic) and [@Giulbia](https://github.com/giulbia).
The idea is to bring together all the components to build a system serving prediction in realtime.
From exploring the data with interactive tools like [Ai Platform](https://cloud.google.com/ai-platform/) 
to deploying streaming application in the cloud with [Confluent Cloud](https://www.confluent.io/confluent-cloud/)
the EDML project discuss the important points leading to model serving automation.

## Why?
In order to share the same schemas between the modules:
- [edml-replay](https://github.com/DivLoic/event-driven-ml/edml-replay/): app generating the validation and test data points
- [edml-serving](https://github.com/DivLoic/event-driven-ml/edml-serving/): app creating the prediction
- [edml-scoring](https://github.com/DivLoic/event-driven-ml/edml-scoring/): app join the prediction to the actual result

This module gather all the avro schemas. After the schema validation, the module generates plain java objects, 
add thel into a [maven package](https://github.com/DivLoic/edml-schema/packages/176609) to be used by all the listed module.

```xml
<dependency>
  <groupId>fr.xebia.gbildi</groupId>
  <artifactId>edml-schema</artifactId>
  <version>0.1.0-SNAPSHOT</version>
</dependency>
```

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
- [EDML: Event-driven ml slides (2019)](https://speakerdeck.com/loicdivad/event-driven-machine-learning)
- [EDMl on Confluent Webinar](https://www.confluent.io/online-talks/event-driven-machine-learning-avec-publicis-sapient/)
- [EDML: Event-driven ml slides (2020)](https://speakerdeck.com/giulbia/event-driven-machine-learning-6822798c-54ea-4db3-b348-b536b3ec5d9e)
