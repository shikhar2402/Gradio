# Gradio

<p align="center">
  <img src="src/gradio.jpg" />
</p>

---

# Gradio: Build Machine Learning Web Apps — in Python

## This repository aims to use latest machine learning models like stable diffusion, vit and language models to integrate with gradio library. 


Gradio is an open-source Python library that is used to build machine learning and data science demos and web applications.

With Gradio, you can quickly create a beautiful user interface around your machine learning models or data science workflow and let people "try it out" by dragging-and-dropping in their own images,
pasting text, recording their own voice, and interacting with your demo, all through the browser.


Gradio is useful for:

- **Demoing** your machine learning models for clients/collaborators/users/students.

- **Deploying** your models quickly with automatic shareable links and getting feedback on model performance.

- **Debugging** your model interactively during development using built-in manipulation and interpretation tools.

## Quickstart

**Prerequisite**: Gradio requires Python 3.7 or higher, that's all!

### What Does Gradio Do?

One of the *best ways to share* your machine learning model, API, or data science workflow with others is to create an **interactive app** that allows your users or colleagues to try out the demo in their browsers.


---

_For questions & feedback, please reach out to shikhar.saxena24@gmail.com!

---

 import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from pyspark.sql import SparkSession

# Get job arguments
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

# Initialize Spark and Glue contexts
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

# Set Parquet file paths
input_path = "s3://your-input-bucket/input-path/"
output_path = "s3://your-output-bucket/output-path/"

# Read Parquet files from S3
datasource = glueContext.create_dynamic_frame.from_catalog(database = "your_database", table_name = "your_table")

# Convert DynamicFrame to DataFrame
data_frame = datasource.toDF()

# Truncate the 'description' column
data_frame = data_frame.withColumn("description", data_frame["description"].substr(1, 100))

# Convert DataFrame back to DynamicFrame
transformed_dynamic_frame = DynamicFrame.fromDF(data_frame, glueContext, "transformed_dynamic_frame")

# Write the transformed DynamicFrame back to S3 in Parquet format
glueContext.write_dynamic_frame.from_catalog(transformed_dynamic_frame, database = "your_database", table_name = "output_table", redshift_tmp_dir=output_path)

# Commit the job
job.commit()
