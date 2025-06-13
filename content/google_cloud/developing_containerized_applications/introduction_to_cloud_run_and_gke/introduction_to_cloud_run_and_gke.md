◀️ [Home](../../../../README.md)

## Introduction to Cloud Run and Google Kubernetes Engine
### Cloud Run
Cloud Run is a fully managed compute platform that lets you deploy and run containers directly on top of Google's scalable infrastructure. If you can build a container image of your application code written in any language, you can deploy the application on Cloud Run.

You can use the [source-based deployment](https://cloud.google.com/run/docs/deploying-source-code) option that builds the container for you, when developing your application in Go, Node.js, Python, Java, .NET Core, or Ruby. Cloud Run works well with other services on Google Cloud, so you can build full-featured applications without spending too much time operating, configuring, and scaling your Cloud Run service.

![Cloud Run developer workflow](image.png)

The Cloud Run developer workflow is a three-step process:
1. First, you write your application using your favorite programming language. This application should start a server that listens for web requests.
2. Second, you build and package your application into a container image.
3. Finally, you deploy the container image to Cloud Run. 

Once you’ve deployed your container, you get a unique HTTPS URL. Cloud Run then starts your container on demand to handle requests, and ensures that all incoming requests are handled by dynamically adding and removing containers. Cloud Run is serverless. That means that you, as a developer, can focus on building your application, and not on building and maintaining the infrastructure that powers your application. 

![Cloud Run source-based workflow](image-1.png)

For some use cases, a container-based workflow is great, because it gives you a great amount of transparency and flexibility. If you build the container image yourself, you decide exactly which files are packaged in your container image. However, building an application is hard enough already, and adding containerization adds additional work and responsibilities. If you’re just looking for a way to turn source code into an HTTPS endpoint by creating and deploying a containerized application in a secure, well-configured, and consistent manner, you can use Cloud Run. With Cloud Run, you can use a container-based workflow, and a source-based workflow. If you use the source-based approach, you deploy your source code, instead of a container image. Using Buildpacks, Cloud Run then builds your source, and packages the application along with its dependencies into a container image for you.

Cloud Run supports secure HTTPS requests to your application. On Cloud Run, your application can either run continuously as a service or as a job. Cloud Run services respond to web requests, or events, while jobs perform work and quit when that work is completed.

- Provisions a valid TLS certificate, and other configuration to support HTTPS requests.
- Handles incoming requests, decrypts, and forwards them to your application.

Cloud Run expects your container to listen on port number 8080 to handle web requests. The port number 8080 is a configurable default, so if this port is unavailable to your application, you can change the application’s configuration to use a different port.
You don’t need to provide an HTTPS server, Google’s infrastructure handles that for you.

One major advantage of Cloud Run is that it runs containers. This means you can develop your applications in any programming language and run them on Cloud Run, as long as they can be compiled to a 64-bit Linux binary, and packaged in a container image.

![Pricing Model](image-2.png)

In summary:
- Cloud Run is a managed serverless product on Google Cloud that runs and autoscales containers on-demand.
- You can deploy any containerized application that handles web requests.
- You can employ a source-based or container-based workflow.
- Cloud Run handles HTTPS requests to your application.
- With the Cloud Run pricing model, you only pay for what you use.

#### Features and use cases of Cloud Run
##### Serving a REST API with Cloud Run
A common use case for Cloud Run is to deploy a service that provides a REST API. You can use the service to provide an API, a website, or a web application. If required, you can connect the service to a database to persist data handled by the API or web application.

##### An ecommerce site on Cloud Run
You can build a more complex public website, for example, an e-commerce site on Cloud Run. In this case, you could also:
- Enable Cloud CDN to improve performance,
- Add Google Cloud Armor to filter malicious inbound traffic using content-based policies. In the backend, you can connect with a relational database, a Redis store for user sessions, and connect with third-party APIs.

##### Microservices on Cloud Run
You can deploy and run an application that is composed of many microservices on Cloud Run.
Services on Cloud Run can communicate with each other using REST APIs or gRPC. Using Pub/Sub, you can send and receive asynchronous messages between services with guaranteed delivery. Pub/Sub is well integrated with Cloud Run using push subscriptions. Pub/Sub forwards and optionally authenticates messages as HTTP requests to the endpoint of your Cloud Run service.

##### Event processing on Cloud Run
Cloud Run integrates with various Google Cloud services such as Cloud Storage, Cloud Build, Pub/Sub, Eventarc, and others that generate events from your cloud infrastructure. This enables you to build event processing workflows with Cloud Run. 

![Event processing on Cloud Run](image-3.png)

When an image of a medical scan is uploaded to Cloud Storage, a Cloud Run service is triggered to process the scanned image and convert it into a modern format. The service then pushes a message to Pub/Sub that triggers another Cloud Run service to label and watermark the converted image, and another VM application that detects anomalies in the scan data. Both services generate output that is stored back in Cloud Storage.

##### Scheduling a Cloud Run service with Cloud Scheduler
You can use Cloud Scheduler to securely trigger a Cloud Run service on a schedule. Cloud Scheduler is a fully-managed cron job scheduler. Some examples of scheduled services include generating invoices, or rebuilding a search index. The limitation of running a scheduled job in the container itself is that the lifetime of a container is only guaranteed while it’s handling requests. If you schedule tasks on a container to run later, the container might be shut down or stopped by the time the task has to run. Note that the Cloud Run service must complete its task within the configured request timeout.

##### Design HA applications with Cloud Run
Cloud Run helps you design applications that are highly available. To support high availability, Cloud Run provides:
- Incremental application updates
- Autoscaling
- Load Balancing across zones and regions

###### Incremental application updates with service revisions
A common cause of service disruptions is often application updates, which affect the availability of your application. On Cloud Run, each deployment of your container image to a service creates a new revision. A service revision is immutable and cannot be modified. If you make a change to your application and deploy it, Cloud Run creates a new revision of your service. A service revision consists of:
- Your container image, and
- The service configuration that includes settings such as environment variables, memory limits, and other configuration values.

You can reduce the impact of request processing failures by splitting request traffic between the new and previous revisions of your service, by specifying the percentage of requests that should be sent to the new revision. This lets you roll back to a previous stable revision if there is a high rate of request failures, or gradually send 100% of request traffic to the new revision.

###### Automatic scaling with Cloud Run
To maintain the capacity to handle incoming requests to your service, Cloud Run automatically increases the number of container instances of a service revision when necessary. This feature is known as autoscaling. Requests to a service revision are distributed across the group of container instances.
- If all container instances are busy, Cloud Run adds additional instances.
- When demand decreases, Cloud Run stops sending traffic to some instances
and shuts them down.
- Note that a container instance can receive many requests at the same time.
With the concurrency setting, you can set the maximum number of requests that can be sent in parallel to a given container instance. In addition to the rate of incoming requests to your service, the number of container instances is impacted by:
- The CPU utilization of existing instances when they are processing requests (with a target of 60% of utilization).
- The maximum concurrency setting.
- The minimum and maximum number of container instances setting.

###### Regions and zones
Cloud Run is a regional service that lets you choose a region where your containers are deployed. A region is a specific geographical location where your Google Cloud resources are hosted. A region consists of three or more zones. Zones and regions are logical abstractions of underlying physical resources that are provided in one or more data centers. An example of a region is us-central1 in Iowa, North America. A zone is a deployment area for cloud resources within a region. Zones are considered to single failure domains within a region. For high availability, Cloud Run distributes your containers over multiple zones in a region, making your application resilient against the failure of a zone.

##### Considerations when using Cloud Run
If you deploy a service that scales up to many container instances, you will incur costs for running those containers. To limit the number of instances during autoscaling, you can set the maximum number of container instances for your Cloud Run service.
- If your Cloud Run service scales up to many container instances in a short period of time, your downstream systems might not be able to handle the additional traffic load. You’ll need to understand the throughput capacity of those downstream systems when configuring your Cloud Run service.
- As part of your application modernization strategy, you’ll need to create a migration plan and use tools to migrate VM-based workloads into containers that will run on Cloud Run or Google Kubernetes Engine.

<br>

In summary:
- Cloud Run runs and autoscales your application on-demand.
- Use Cloud Run for applications that serve web requests, including microservices, event processing workflows, and scheduled tasks or jobs.
- Automatic scaling, incremental application updates, and built-in load balancing help you build highly available applications.
- Cloud Run is designed to make developers more productive.