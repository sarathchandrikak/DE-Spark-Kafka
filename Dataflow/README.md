# 🚩 GCP Dataflow

Google Cloud Dataflow is a fully managed, serverless, fast, cost effective data processing service that allows developers to build and execute data pipelines for both batch and stream processing. 
It removes operational overhead by automating the infrastructre provisioning and auto scaling as data grows. 

 -> Read data from source\
 -> Transform the data\
 -> Write the data back to the sink

It provides portability with the processing pipeline created using open source apache beam library and dataflow job that runs on virtual machines. 


Apache Beam is collection of SDKs for building streaming data processing pipelines 
Cloud Dataflow is fully managed and integrated service for executing optimized data processing pipelines
Dataflow natively integrates with Cloud Storage, Pub/Sub, Bigquery
Connectors available for Bigtable and Kafka

### 🚩 Core Concepts
   * PCollection - Distributed data set, an object that gets inputted into pipeline and gets as output
   * PCollections are spread across multiple nodes/machines
   * Rows of PCollection gets distributed across worker nodes
   * Each machine handles different subset of data and more the data more the worker nodes
   * Element - Single entry of a row
   * ParDo - a type of transform applied to individual elements of a PCollection (filtering / extracting elements)
   * DoFn - Custom logic, that gets applied to each element
   * Flatten merges multiple PCollections of same type into single PCollection

#### 🚩 DataFlow Windows 
  * Mostly used to process chunk of realtime streaming data
  * Tumbling Windows - fixed duration windows, data is divided into distinct non overlapping intervals of time and sequential
  * Hopping/Sliding Windows - fixed duration windows, overlapping intervals of time -> depends on size of the hop, fixed hop frequency
  * Session based Windows - Defined by gap in the data, if data gap is more than a defined interval, new window starts, dynamic length

#### 🚩 Watermarks
  * Timestamps that keep track of progress in pipeline
  * If step fails or stops, watermark just waits and doesn't move forward
  * Allows system to process late arriving data without skipping or loosing any important information
  * Watermarks are based on event time - when the event has occurred, not when it got processed/delays

#### 🚩 Triggers
  * Essential in determining when data is embedded in streaming pipelines
  * Conditions that determine when the aggregated results of data should be omitted
  * Triggers can be used to refire-window, updates with new data
  * Event time trigger - Fired when watermark reaches certain point
  * Processing time trigger - Fired based on real world clock time
  * Data driven trigger - Fired when a certain number of data records are processed



