# Spark Machine Learning

The team deploys machine learning models and services that fall mainly into three categories:  
- **scikit-learn**
- **Spark MLlib**
- **TensorFlow**

This section focuses on **machine learning models in Apache Spark**.  
Among these options, **Spark MLlib** is often the easiest to set up and scale when deploying models as services — particularly when handling large datasets or high volumes of requests.

However, models that are used **infrequently** or on **smaller datasets** generally gain **little advantage** from Spark unless the underlying data already resides in a **distributed file system** such as HDFS or cloud object storage.  

Here, we’ll explore how the team uses **Spark MLlib** to build and deploy scalable machine learning services.

## The Design Thinking Process

### Understanding “Scale” in Design Thinking

When approaching model deployment through the **Design Thinking** framework, one key question arises:  
> “What does scale mean for our users and solution?”

To determine this, start by examining the **users** and **personas** identified in the **hill statements** guiding your project.  

If your personas represent **large populations** — perhaps **millions of users** — then scalability must be a top design and engineering consideration. You’ll need to collaborate with technical teams to ensure that your machine learning solution can handle:
- High volumes of concurrent users
- Large datasets
- Real-time or near-real-time inference

---

### Considering Model Complexity

The **type of model** you deploy has a major impact on scalability and performance requirements.  
During the **Prototype** and **Test** phases of design thinking, you will likely have experimented with multiple algorithms.

Examples:
- **Neural networks** or **deep learning models** → Require significant computational resources (GPUs, distributed clusters)
- **Logistic regression** or **decision trees** → Lighter and more efficient for smaller-scale tasks

These considerations are critical during **playback sessions** with architects and engineers, where design, infrastructure, and performance expectations are aligned.

---

### Balancing Performance and User Expectations

Model complexity and large-scale user bases directly affect **model performance**.  
During prototyping, it’s important to ask:
- What are users expecting in terms of **response time**?
- How fast should predictions be delivered under load?

To accurately measure these expectations, your prototype models should be deployed on **scalable platforms** such as **Apache Spark**.  

By leveraging Spark’s distributed computing capabilities, teams can:
- Parallelize data processing and model inference
- Scale horizontally across multiple nodes
- Meet user performance targets efficiently

---

## Summary

The **team’s approach** to Spark-based machine learning demonstrates how enterprise data science can integrate:
- **Design thinking** principles (user-centric iteration)
- **Technical scalability** (Spark MLlib and distributed data processing)
- **Deployment readiness** (models as scalable, production-grade services)

Apache Spark provides the infrastructure needed to **test, refine, and scale** machine learning models — ensuring they perform efficiently under real-world user demands.

[Previous Page](Getting-Started-With-Flask.md) -  [Next Page](Spark-Pipelines.md)
