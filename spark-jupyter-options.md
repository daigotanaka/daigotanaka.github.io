## Options for Spark Cluster + Notebook setup on Cloud

![](https://raw.githubusercontent.com/daigotanaka/essays/master/images/spark-jupyter-kubernetes.png)

### Motivation

More people join to use Apache Spark for complex ETL (Extract Transform Load)
jobs and building Machine Learning models. Data Scientist may use Spark
interactively with Jupyter Notebook or similar notebook environment coding
in Python with PySpark or in R with Sparklyr.

If you are a Data Scientist who wants to try out Spark for the first time,
(and your organization does not provide such set up for you), you may
struggle with the very first step: How would one set up a Spark cluster on
a cloud computing service?

### Options

Today, we do not have to have thousands of dollars of budget to run a small
project and we have several options depends on the data engineering resource
you have:

1. Use a hosted service like [Databricks](https://databricks.com) or
[Qubole](https://qubole.com)
2. Use Elastic Map Reduce (EMR) on Amazon Web Services (AWS)
3. Manually set up cluster

Note that I didn't include services like Hortonworks or Cloudera as they seem
to require more commitment before you can try out. Also, the options above are
all targeting to deploy on AWS. Microsoft
[announced](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-overview)
Spark on HDInsight and it should allow you to run sample projects for free
with their notebook features.

### Option 1: Hosted services

This is probably the quickest path to a notebook environment connected to
Spark clusters. Both Databricks and Qubole provides a tryout period or
community edition to run a small cluster.

Databricks offers a
[Community Edition](https://databricks.com/try-databricks) with Mini 6GB
memory cluster. The first tier commercial plan costs $0.40~ plus the cost of
running AWS instances per hour.
([Pricing Details](https://databricks.com/product/pricing))

Qubole offers a tryout period. You can start up to 4 clusters. Pricing
information is not available on the web page. Qubole seems to be preparing its
Community Edition as I write this.

For anybody who does not mind spending extra per hour, those are great options.
You can spin up a cluster with a click and start coding in the notebook. It is
also good if you need to manage the cluster usage of a team or an organization.
The notebook is their own versions but the interface is very similar. You can
connect to AWS S3 of your account to work with your own data.

### Option 2: EMR

If you don't have to manage multiple team members' cluster usage, spinning up
a cluster of EC2 instances via EMR is one of the cheapest ways to go.
After you create an
[AWS account](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html)
and create a security key pair, you could set up a running Spark cluster and
a Jupyter Notebook interface within an hour by following
[this instruction](https://aws.amazon.com/blogs/big-data/running-jupyter-notebook-and-jupyterhub-on-amazon-emr/).

In order to access the Jupyter Notebook, you need to set up an
[ssh tunnel](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)
and [configure proxy settings on the web browser](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html).
It is not as scary as it sounds though.

The Jupyter notebook from the above instruction comes with various examples
including PySpark and Sparklyr. The good thing about this environment is
that you can save the work in the notebook directly to S3 so it won't be
lost when the cluster is terminated.

### Option 3: Manual set up

For starting a cluster, "manual" setups sound a lot of work. But
[Kubernetes](https://kubernetes.io/) is making our life really easy to
orchestrate the Docker containers. I tried this option. After finding the
right resource, it would not be so hard if you are familiar with Docker.
My struggle was to find examples of the Docker containers for Spark Master/Workers,
Jupyter, and Kubernetes configuration files. Luckily,
[pipeline.io](http://pipeline.io) by [Chris Fregly](https://www.linkedin.com/in/cfregly)
worked so hard to provide everything we need. I just had to follow the
step-by-step command line from
[this tutorial](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Pipeline-AWS).

pipeline.io comes with other great open source tools to visualize and manage
the status of the cluster. It is also scoping to deploy the model into the
production. The Community Edition of pipeline.io is completely free. You just
need to pay for the cost of your AWS usage. pipeline.io seems to be preparing
an [Advanced Edition](http://pipeline.io/products/#advanced-edition).

This option certainly requires engineering knowledge such as Docker. But it is
the most flexible option with the possibility to streamline the process and
deploy the model to the production.

Another advantage of this option is that you can run the same docker containers
on the local computer for a small scale test run. Get the code done and tested
locally with a small data set could save a lot of money at the end.
