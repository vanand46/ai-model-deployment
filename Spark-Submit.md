# spark-submit

A Spark cluster can be managed by different cluster managers, such as:

- Apache Mesos  
- Hadoop YARN  
- Spark on Kubernetes  

You can submit Spark applications to any of these cluster environments using the `spark-submit` command and an appropriate script. Because Spark clusters often serve multiple users and applications, the cluster management and scheduling layer is critical for monitoring, resource allocation, and job control.

---

## Example: Using spark-submit

You might create a script file called `example-spark-submit.sh` like this:

```bash
#!/usr/bin/env bash

spark-submit \
  --master yarn \
  --deploy-mode cluster \
  $@
```
In this script:
- The `"$@"` placeholder represents all arguments passed to the script, so it appends them to the `spark-submit` invocation
- For example, to submit a Python script example-script.py using the shell script, then run

```bash
./example-spark-submit.sh example-script.py --arg1 value
```

### Permissions
Ensure that your submission script is executable.For example:

```bash
chmod +x example-spark-submit.sh
```

## Further Reading
Refer to the Spark Application Submission Guide for full documentation on spark-submit options and patterns.

[Previous Page](Apache-Spark-Hands-On.md) -  [Next Page](Container-Intro.md)
