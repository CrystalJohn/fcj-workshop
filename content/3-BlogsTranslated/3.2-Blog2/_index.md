---
title: "Build end-to-end Apache Spark pipelines with Amazon MWAA, Batch Processing Gateway, and Amazon EMR on EKS"
date: 2025-05-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

## By: Avinash Desireddy and Suvojit Dasgupta

**Date:** May 1, 2025

**Topics:** [Amazon EMR on EKS](https://aws.amazon.com/emr/features/eks/), [Amazon Managed Workflows for Apache Airflow (Amazon MWAA)](https://aws.amazon.com/managed-workflows-for-apache-airflow/), [AWS Big Data](https://aws.amazon.com/big-data/), [Intermediate Level (200)](https://aws.amazon.com/blogs/big-data/category/learning-level/intermediate-200/), [Open Source](https://aws.amazon.com/opensource/)

---

[Apache Spark](https://spark.apache.org/) workloads running on [Amazon EMR on EKS](https://aws.amazon.com/emr/features/emr-on-eks/) are foundational to many modern data platforms. EMR on EKS provides benefits by offering a managed Spark environment, integrating seamlessly with other AWS services, and your organization's existing Kubernetes-based deployment patterns.

Data platforms processing large-scale data volumes often require multiple EMR on EKS clusters. In the post [Use Batch Processing Gateway to automate job management in multi-cluster Amazon EMR on EKS environments](https://aws.amazon.com/blogs/big-data/use-batch-processing-gateway-to-automate-job-management-in-multi-cluster-amazon-emr-on-eks-environments/), we introduced **Batch Processing Gateway (BPG)** as a solution for managing Spark workloads across these clusters. Although BPG provides foundational functionality for workload distribution and routing support for Spark jobs in multi-cluster environments, enterprise data platforms demand additional features for a comprehensive data processing pipeline.

This post shows how to enhance the multi-cluster solution by integrating [Amazon Managed Workflows for Apache Airflow](https://aws.amazon.com/mwaa/) (Amazon MWAA) with BPG. By using Amazon MWAA, we add job scheduling and orchestration capabilities, enabling you to build a comprehensive end-to-end Spark-based data processing pipeline.

---

## Solution overview

Consider HealthTech Analytics, a healthcare analytics company managing two distinct data processing workloads. Their Clinical Insights Data Science team processes sensitive patient outcome data, requiring HIPAA compliance and dedicated resources. Meanwhile, the Digital Analytics team handles website interaction data with more flexible requirements. As their operations have grown, they face increasing challenges in managing these diverse workloads efficiently.

The company needs to maintain strict separation between processing protected health information (PHI) and non-PHI data, while addressing different cost center requirements. The Clinical Insights team runs critical end-of-day batch processes that need guaranteed resources, whereas the Digital Analytics team can use cost-optimized spot instances for their variable workloads. Additionally, data scientists from both teams require environments for experimentation and prototyping on demand.

This scenario is an ideal use case for implementing a data pipeline using Amazon MWAA, BPG, and multiple EMR on EKS clusters. The solution needs to route different Spark workloads to appropriate clusters based on security requirements and cost profiles, while maintaining necessary isolation and compliance controls. To efficiently manage such an environment, we need a solution that maintains clear separation between application and infrastructure management concerns, and connects multiple components together into a robust pipeline.

Our solution involves integrating Amazon MWAA with BPG through an [Airflow custom operator](https://airflow.apache.org/docs/apache-airflow/stable/howto/custom-operator.html) for BPG called the **BPGOperator**. This operator encapsulates the infrastructure management logic required to interact with BPG. The `BPGOperator` provides a clean interface for job submission through Amazon MWAA. When invoked, the operator communicates with BPG, which then routes Spark workloads to available EMR on EKS clusters based on predefined routing rules.

The following architecture diagram illustrates the components and their interactions.

![Solution architecture](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-architecture.png)

The solution works through the following steps:

1. **Amazon MWAA** runs scheduled [DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html) using the `BPGOperator`. Data engineers create DAGs using this operator, requiring only the Spark application configuration file and basic scheduling parameters.
2. **`BPGOperator`** validates and submits jobs to the BPG endpoint `POST:/apiv2/spark`. It handles all HTTP communication details, manages authentication tokens, and provides secure transmission of job configurations.
3. **BPG** routes submitted jobs to EMR on EKS clusters based on predefined routing rules. These rules are centrally managed through BPG's configuration, enabling rule-based workload distribution across multiple clusters.
4. **`BPGOperator`** monitors job status, logs, and handles execution retries. It polls the BPG job status endpoint `GET:/apiv2/spark/{subID}/status` and streams logs to Airflow by polling the `GET:/apiv2/log` endpoint every second. The BPG log endpoint retrieves the latest log information directly from the Spark Driver Pod.
5. DAG execution proceeds to subsequent tasks based on job completion status and defined dependencies. The `BPGOperator` communicates job status through Airflow's built-in task communication system, enabling complex workflow orchestration.

Refer to BPG's [REST API interface](https://github.com/aws-samples/amazon-emr-on-eks-batch-processing-gateway/blob/main/docs/api-spec/openapi.yaml) documentation for more details.

This architecture provides several key benefits:

* **Separation of concerns** – Data Engineering and Platform Engineering teams in enterprise organizations often maintain distinct responsibilities. The modular design in this solution allows platform engineers to configure the `BPGOperator` and manage EMR on EKS clusters, while data engineers maintain DAGs.
* **Centralized code management** – The `BPGOperator` encapsulates all core functionality needed for Amazon MWAA DAGs to submit Spark jobs through BPG into a single, reusable Python module. This centralization minimizes code duplication across DAGs and improves maintainability by providing a standardized interface for job submission.

---

## Airflow custom operator for BPG

An [Operator](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/operators.html) in Airflow is a template for a predefined [Task](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/tasks.html) that you can define declaratively inside your DAG. Airflow provides many built-in operators such as `BashOperator` to run bash commands, `PythonOperator` to run Python functions, and `EmrContainerOperator` to submit a new job to an EMR on EKS cluster. However, there is no built-in operator that performs all the steps required for Amazon MWAA integration with BPG.

Airflow allows you to create new operators to fit your specific requirements. This type of operator is called a **custom operator**. A custom operator encapsulates custom infrastructure-related logic into a single, maintainable component. Custom operators are created by extending the `airflow.models.baseoperator.BaseOperator` class. We developed and open-sourced an Airflow custom operator for BPG called **`BPGOperator`**, which performs the steps required to provide seamless integration of Amazon MWAA with BPG.

The following class diagram provides a detailed view of the `BPGOperator` implementation.

![BPGOperator class diagram](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-bpg-operator-class-diagram.png)

When a DAG includes a `BPGOperator` task, the Amazon MWAA instance triggers the operator to submit a job request to BPG. The operator typically performs the following steps:

1. **Job initialization** – The `BPGOperator` prepares the job payload, including input parameters, configuration, connection details, and other metadata that BPG requires.
2. **Job submission** – The `BPGOperator` handles HTTP POST requests to submit jobs to BPG endpoints with the provided configurations.
3. **Job execution monitoring** – The `BPGOperator` checks the job status, polling BPG until the job completes successfully or fails. The monitoring process includes handling various job states, managing timeout scenarios, and responding to errors that occur during job execution.
4. **Job completion handling** – Upon completion, the `BPGOperator` logs the job results, records relevant details, and can trigger subsequent tasks based on execution results.

The following sequence diagram illustrates the interaction flow between the Airflow DAG, `BPGOperator`, and BPG.

![Sequence diagram](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-bgp-operator-sequence-diagram.png)

---

## Deploying the solution

In the remainder of this post, you will deploy an end-to-end pipeline to run Spark jobs across multiple EMR on EKS clusters. You will start by deploying the common components that form the foundation for building pipelines. Next, you will deploy and configure BPG on an EKS cluster, followed by deploying and configuring the `BPGOperator` on Amazon MWAA. Finally, you will run Spark jobs across multiple EMR on EKS clusters from Amazon MWAA.

To streamline the installation process, we have automated the deployment of all infrastructure components required for this post, so you can focus on the essential aspects of job submission to build an end-to-end pipeline. We provide detailed information to help you understand each step, simplifying setup while maintaining the learning experience.

To demonstrate the solution, you will create three clusters and one Amazon MWAA environment:

* Two EMR on EKS clusters: `analytics-cluster` and `datascience-cluster`
* One EKS cluster: `gateway-cluster`
* One Amazon MWAA environment: `airflow-environment`

The `analytics-cluster` and `datascience-cluster` serve as data processing clusters running Spark workloads, `gateway-cluster` hosts BPG, and `airflow-environment` hosts Airflow for job orchestration and scheduling.

You can find the source code in the [GitHub repository](https://github.com/aws-samples/sample-mwaa-bpg-emr-on-eks-spark-pipeline).

### Prerequisites

Before deploying this solution, make sure the following prerequisites are met:

* Access to a valid AWS account
* [AWS Command Line Interface](https://aws.amazon.com/cli/) (AWS CLI) [installed](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) on your local machine
* Git, [Docker](https://docs.docker.com/get-docker/), [eksctl](https://eksctl.io/), [kubectl](https://kubernetes.io/docs/tasks/tools/), [Helm](https://helm.sh/docs/intro/install/), and [jq](https://stedolan.github.io/jq/download/) utilities installed on your local machine
* Permissions to create AWS resources
* Familiarity with Kubernetes, Amazon MWAA, Apache Spark, [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), and Amazon EMR on EKS

### Common infrastructure setup

This step handles network infrastructure setup, including virtual private cloud (VPC) and subnets, along with configuring [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) roles, [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) storage, [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/) (Amazon ECR) repositories for BPG images, [Amazon Aurora PostgreSQL-Compatible Edition](https://aws.amazon.com/rds/aurora/postgresql-features/) database, Amazon MWAA environment, and both EKS and EMR on EKS clusters with a preconfigured Spark operator. With this infrastructure provisioned automatically, you can focus on the subsequent steps without being bogged down by basic setup tasks.

Clone the repository to your local machine and set two environment variables. Replace `<AWS_REGION>` with the AWS Region where you want to deploy these resources.

```bash
git clone https://github.com/aws-samples/sample-mwaa-bpg-emr-on-eks-spark-pipeline.git
cd sample-mwaa-bpg-emr-on-eks-spark-pipeline
export REPO_DIR=$(pwd)
export AWS_REGION=<AWS_REGION>
```

Run the following script to create the common infrastructure:

```bash
cd ${REPO_DIR}/infra
./setup.sh
```

To verify successful infrastructure deployment, navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home), open your stack, and check the **Events**, **Resources**, and **Outputs** tabs to see the completion status, details, and list of created resources.

You have completed setting up the common components that form the foundation for the rest of the deployment.

### Batch Processing Gateway setup

This section builds the Docker image for BPG, deploys the helm chart on the EKS cluster `gateway-cluster`, and exposes the BPG endpoint using a Kubernetes service of type `LoadBalancer`. Complete the following steps:

1. Deploy BPG on the EKS cluster `gateway-cluster`:

```bash
cd ${REPO_DIR}/infra/bpg
./configure_bpg.sh
```

2. Verify the deployment by listing pods and viewing the pod logs:

```bash
kubectl get pods --namespace bpg
kubectl logs <BPG-PODNAME> --namespace bpg
```

Review the logs and confirm there are no errors or exceptions.

3. Exec into the BPG pod and verify the health check:

```bash
kubectl exec -it <BPG-PODNAME> -n bpg -- bash
curl -u admin:admin localhost:8080/skatev2/healthcheck/status
```

The `healthcheck` API will return a successful response of `{"status":"OK"}`, confirming successful BPG deployment on the EKS cluster `gateway-cluster`.

We have successfully configured BPG on the `gateway-cluster` and set up EMR on EKS for both the `datascience-cluster` and `analytics-cluster`. This is the point where we stopped in the previous blog. In the following steps, we will configure Amazon MWAA with the `BPGOperator`, then write and submit DAGs to illustrate an end-to-end Spark-based data pipeline.

### Configuring the Airflow operator for BPG on Amazon MWAA

This section configures the `BPGOperator` plugin on the Amazon MWAA environment `airflow-environment`.

1. Configure the `BPGOperator` on Amazon MWAA:

```bash
cd ${REPO_DIR}/bpg_operator
./configure_bpg_operator.sh
```

2. On the Amazon MWAA console, navigate to the `airflow-environment` environment.
3. Choose **Open Airflow UI**, and in the Airflow interface, choose the **Admin** dropdown menu and choose **Plugins**.
4. You will see the `BPGOperator` plugin listed in the Airflow interface.

![BPGOperator Plugin](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-mwaa-plugin.png)

### Configuring Airflow connections for BPG integration

This section walks you through setting up Airflow connections that enable secure communication between the Amazon MWAA environment and BPG. The `BPGOperator` uses the configured connection to authenticate and interact with BPG endpoints.

Run the following script to configure the Airflow connection `bpg_connection`:

```bash
cd $REPO_DIR/airflow
./configure_connections.sh
```

In the Airflow interface, choose the **Admin** dropdown menu and choose **Connections**. You will see `bpg_connection` listed.

![Airflow Connections](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-airflow-connection.png)

### Configuring Airflow DAG to run Spark jobs

This step configures an Airflow DAG to run a sample application. Specifically, we will submit a DAG containing multiple sample Spark jobs using Amazon MWAA to EMR on EKS clusters using BPG. Please wait a few minutes for the DAG to appear in the Airflow interface.

```bash
cd $REPO_DIR/jobs
./configure_job.sh
```

### Triggering the Amazon MWAA DAG

In this step, we trigger the Airflow DAG and observe the job execution behavior, including reviewing the Spark logs in the Airflow interface:

1. In the Airflow interface, review the DAG `MWAASparkPipelineDemoJob` and choose the play icon to trigger the DAG.

![Trigger DAG](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-dags.png)

2. Wait for the DAG to complete successfully.
3. When the DAG completes successfully, you will see `Success:1` under the **Runs** column.
4. In the Airflow interface, find and choose the DAG `MWAASparkPipelineDemoJob`.

![Successful DAG](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-dag-graphview.png)

5. On the **Graph** tab, choose any task (for example, we choose the task `calculate_pi`) and then choose **Logs**.

![View Logs](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-mwaa-job-logs.png)

6. View the Spark logs in the Airflow interface.

---

## Migrating existing Airflow DAGs to use BPG

In enterprise data platforms, a typical data pipeline consists of Amazon MWAA submitting Spark jobs to multiple EMR on EKS clusters using the `SparkKubernetesOperator` and an `Airflow Connection` of type Kubernetes. An Airflow Connection is a set of parameters and credentials used to establish communication between Amazon MWAA and external systems or services. A DAG references the connection name and connects to the external system.

The following diagram shows the typical architecture.

![Typical architecture](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-mwaa-existing.png)

In this setup, Airflow DAGs typically use `SparkKubernetesOperator` and `SparkKubernetesSensor` to submit Spark jobs to a remote EMR on EKS cluster using `kubernetes_conn_id=<connection_name>`.

```python
# Submit Spark-Pi job using Kubernetes connection
submit_spark_pi = SparkKubernetesOperator(
    task_id='submit_spark_pi',
    namespace='default',
    application_file=spark_pi_yaml,
    kubernetes_conn_id='emr_on_eks_connection_[1|2]',  # Connection ID defined in Airflow
    dag=dag)
```

To migrate the infrastructure to BPG-based infrastructure without affecting environment continuity, we can deploy a parallel infrastructure using BPG, create a new Airflow Connection for BPG, and gradually migrate DAGs to use the new connection. By doing so, we will not disrupt the existing infrastructure until the BPG-based infrastructure is fully operational, including migrating all existing DAGs.

The following diagram introduces the transient state where both Kubernetes connections and BPG connections are operational. Blue arrows indicate existing workflow paths, and red arrows represent new BPG-based migration paths.

![Migration state](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-mwaa-bpg-migration.png)

The modified code snippet for the DAG is as follows:

```python
# Submit Spark-Pi job using BPG connection
submit_spark_pi = BPGOperator(
    task_id='submit_spark_pi',
    application_file=spark_pi_yaml,
    application_file_type='yaml',
    connection_id='bpg_connection',  # Connection ID defined in Airflow
    dag=dag)
```

Finally, when all DAGs have been modified to use `BPGOperator` instead of `SparkKubernetesOperator`, you can decommission any remnants of the old workflow. The final state of the infrastructure will look like the following diagram.

![Final state](/images/3-BlogsTranslated/3.2-Blog2/bdb-5005-mwaa-bpg-final-1.png)

Using this approach, we can seamlessly integrate BPG into an environment that currently only uses Amazon MWAA and EMR on EKS clusters.

---

## Cleanup

To avoid incurring future charges from resources created in this walkthrough, clean up your environment after you have completed the steps. You can do this by running the `cleanup.sh` script, which will safely delete all resources provisioned during setup:

```bash
cd ${REPO_DIR}/setup
./cleanup.sh
```

---

## Conclusion

In the post [Use Batch Processing Gateway to automate job management in multi-cluster Amazon EMR on EKS environments](https://aws.amazon.com/blogs/big-data/use-batch-processing-gateway-to-automate-job-management-in-multi-cluster-amazon-emr-on-eks-environments/), we introduced Batch Processing Gateway as a solution for routing Spark workloads across multiple EMR on EKS clusters. In this post, we demonstrated how to enhance this foundation by integrating BPG with Amazon MWAA. Through our custom `BPGOperator`, we showed how to build robust end-to-end Spark-based data processing pipelines while maintaining clear separation of concerns and centralized code management. Finally, we demonstrated how to seamlessly integrate the solution into your existing Amazon MWAA and EMR on EKS data platform without affecting operational continuity.

We encourage you to experiment with this architecture in your own environment, adapting it to fit your unique workloads and operational requirements. By deploying this solution, you can build efficient and scalable data processing pipelines, harnessing the full potential of EMR on EKS and Amazon MWAA. Explore further by deploying the solution in your AWS account while adhering to your organization's security best practices and share your experiences with the AWS Big Data community.

---

## About the authors

**Suvojit Dasgupta** is a Principal Data Architect at AWS. He leads a team of skilled engineers in designing and building scalable data solutions for AWS customers. He specializes in developing and deploying innovative data architectures to solve complex business challenges.

**Avinash Desireddy** is a Cloud Infrastructure Architect at AWS, passionate about building secure data applications and platforms. He has deep experience in Kubernetes, DevOps, and enterprise architecture, helping customers containerize applications, streamline deployments, and optimize cloud-native environments.
