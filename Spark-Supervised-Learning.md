# Spark Supervised Learning

**Apache Spark MLlib** provides a range of **supervised learning algorithms**, particularly for **classification** and **regression** tasks.  
These algorithms are part of Spark’s scalable distributed machine learning framework, designed to handle **large datasets** efficiently.

**Reference:** [Spark MLlib - Supervised Learning](https://spark.apache.org/docs/latest/ml-classification-regression.html)


## Supported Algorithms
Spark MLlib includes several widely used supervised learning algorithms, such as:

- **Random Forests**
- **Gradient-Boosted Trees (GBTs)**
- **Linear Support Vector Machines (SVMs)**
- **Logistic Regression**
- **Decision Trees**
- **Multilayer Perceptrons (Neural Networks)**

Although Spark MLlib offers **fewer algorithms** than **scikit-learn**, it includes many of the **most popular and production-ready methods**.  
Notably, **Random Forests** and **Gradient-Boosted Trees** are among the most robust and frequently used models for real-world applications. Both of these are **ensemble methods** built on top of **decision trees**.

## Example: Random Forest Classification in Spark

Below is a **minimal example** that demonstrates how to train and evaluate a **Random Forest Classifier** using Spark MLlib.

```python
import pyspark as ps
from pyspark.ml import Pipeline
from pyspark.ml.classification import RandomForestClassifier
from pyspark.ml.feature import IndexToString, StringIndexer, VectorIndexer
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

# Initialize Spark Session
spark = (
    ps.sql.SparkSession.builder
    .appName("aavail-churn")
    .getOrCreate()
)

sc = spark.sparkContext

# Load and parse the data file, converting it to a DataFrame
data = spark.read.format("libsvm").load("./work/sample_libsvm_data.txt")

# Index labels, adding metadata to the label column
# Fit on the entire dataset to include all labels in index
labelIndexer = StringIndexer(inputCol="label", outputCol="indexedLabel").fit(data)

# Automatically identify categorical features and index them
# Set maxCategories so features with > 4 distinct values are treated as continuous
featureIndexer = VectorIndexer(
    inputCol="features",
    outputCol="indexedFeatures",
    maxCategories=4
).fit(data)

# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a RandomForest model
rf = RandomForestClassifier(
    labelCol="indexedLabel",
    featuresCol="indexedFeatures",
    numTrees=10
)

# Convert indexed labels back to original labels
labelConverter = IndexToString(
    inputCol="prediction",
    outputCol="predictedLabel",
    labels=labelIndexer.labels
)

# Chain indexers and forest in a Pipeline
pipeline = Pipeline(stages=[labelIndexer, featureIndexer, rf, labelConverter])

# Train model (this also runs the indexers)
model = pipeline.fit(trainingData)

# Make predictions
predictions = model.transform(testData)

# Display example rows
predictions.select("predictedLabel", "label", "features").show(5)

# Evaluate model accuracy
evaluator = MulticlassClassificationEvaluator(
    labelCol="indexedLabel",
    predictionCol="prediction",
    metricName="accuracy"
)

accuracy = evaluator.evaluate(predictions)
print("Test Error = %g" % (1.0 - accuracy))

# Retrieve the RandomForest model
rfModel = model.stages[2]
print(rfModel)  # Summary of the trained model
```

[Previous Page](Spark-Pipelines.md) -  [Next Page](Spark-Pipelines.md)
