##### DataPrep Notes
* It is a tool in the GCP to transform data through UI and simple syntax.
* Transformations are stacked and executed through dataflow.
* easy to catch the outlier through qucik histogram and **enables** to select and operate on the bars in the histogram plot.

##### Dataflow / Apache Beam:
* https://beam.apache.org/documentation/sdks/python-type-safety/ : tells us how in python we can have custom or udf driver transformation
* https://beam.apache.org/releases/pydoc/2.16.0/: python set of apis
  * PCollection
  * PTranform
  * Pipeline
  * PipelineRunner
  * Read 
  * Write
##### Dataproc
 * in either command or through console steps are 2
  * create cluster
   ```gcloud dataproc clusters create example-cluster```
  * submit job
  ```gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPi \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- 1000```
  
