# Triggermesh
Event based flow using Triggermesh

TriggerMesh is an open-source, cloud-native integration platform designed to build event-driven applications. It offers a unified eventing experience, serving as an alternative to AWS EventBridge. TriggerMesh allows you to create and manage event-driven workflows and integrations across multiple cloud services and on-premises systems, facilitating automation and data synchronization【6†source】【7†source】.

Key features of TriggerMesh include:

1. **Centralized Event Handling**: It uses brokers to centralize events from various sources in a standard format based on CloudEvents, allowing for uniform event processing and routing【6†source】.
2. **Event Transformation**: It provides tools to transform event data to meet the requirements of different event consumers, including removing unnecessary fields or adding new metadata【6†source】.
3. **Custom Resources and Controllers**: TriggerMesh operates on Kubernetes, leveraging custom resources and controllers to manage event sources, targets, and workflows declaratively【8†source】.
4. **Flexibility**: It supports running both on Kubernetes for robust, scalable deployments and on Docker for simpler, local development setups【9†source】.
5. **Integration Connectors**: The platform supports a wide range of connectors for popular cloud services and on-premises applications, enabling comprehensive integration capabilities【6†source】.

TriggerMesh aims to increase agility by decoupling event producers and consumers, thereby reducing lead time for development and improving workflow efficiency【9†source】.

For more detailed information, you can visit their [official website](https://www.triggermesh.com) and their [GitHub repository](https://github.com/triggermesh/triggermesh).

**Next Steps:**
**a.** Explore the TriggerMesh documentation to understand how to set up and configure your first event-driven application.
**b.** Look into potential use cases for TriggerMesh in your specific environment to leverage its full capabilities.

----

To deploy TriggerMesh on a Google Kubernetes Engine (GKE) cluster, you need to follow these steps:

1. **Set up your GKE cluster**:
   Ensure you have a running GKE cluster. If you don't have one, you can create it using the Google Cloud Console or the `gcloud` command-line tool.

   ```sh
   gcloud container clusters create triggermesh-cluster --zone us-central1-a
   gcloud container clusters get-credentials triggermesh-cluster --zone us-central1-a
   ```

2. **Install TriggerMesh Custom Resource Definitions (CRDs)**:
   Deploy the TriggerMesh CRDs to your cluster to define the custom resources used by TriggerMesh.

   ```sh
   kubectl apply -f https://github.com/triggermesh/triggermesh/releases/latest/download/triggermesh-crds.yaml
   ```

3. **Deploy the TriggerMesh platform**:
   Apply the TriggerMesh platform manifest to install the necessary components.

   ```sh
   kubectl apply -f https://github.com/triggermesh/triggermesh/releases/latest/download/triggermesh.yaml
   ```

4. **Verify the installation**:
   Check the status of the TriggerMesh components to ensure they are running correctly.

   ```sh
   kubectl get pods -n triggermesh
   ```

   You should see the TriggerMesh pods in the `Running` state.

5. **Configure TriggerMesh components**:
   You can now start configuring event sources, targets, and workflows using the TriggerMesh custom resources. Below is an example of how to create a TriggerMesh event source and target.

   **Example: Creating a Google Cloud Pub/Sub source and a Knative Service as a target**

   ```yaml
   apiVersion: sources.triggermesh.io/v1alpha1
   kind: GoogleCloudPubSubSource
   metadata:
     name: my-pubsub-source
     namespace: default
   spec:
     serviceAccountKey:
       valueFromSecret:
         name: google-credentials
         key: key.json
     topic: projects/YOUR_PROJECT_ID/topics/YOUR_TOPIC_NAME
     sink:
       ref:
         apiVersion: serving.knative.dev/v1
         kind: Service
         name: event-display
   ```

   Apply the resource:

   ```sh
   kubectl apply -f pubsub-source.yaml
   ```

   **Note**: Replace `YOUR_PROJECT_ID` and `YOUR_TOPIC_NAME` with your actual Google Cloud project ID and Pub/Sub topic name.

6. **Monitor and manage your setup**:
   You can use `kubectl` commands to monitor the status of your event sources, targets, and other TriggerMesh resources. For detailed management and further configurations, refer to the TriggerMesh [documentation](https://docs.triggermesh.io).

By following these steps, you should have TriggerMesh successfully deployed on your GKE cluster and be able to start integrating various event sources and targets.

**Next Steps:**
**a.** Explore advanced configurations for custom event routing and transformation.
**b.** Set up monitoring and logging for your TriggerMesh components to ensure they are functioning as expected.

----

Yes, TriggerMesh provides a user interface (UI) designed to simplify the creation and management of event-driven integrations. This low-code UI is beneficial for less technical users, enabling them to define and manage integrations without needing to write extensive code. The UI allows you to visually create event flows, configure sources and targets, and apply transformations and filters to the events【17†source】【20†source】.

To use the TriggerMesh UI:
1. **Set up your GKE cluster** and deploy TriggerMesh as described in the previous instructions.
2. **Access the UI**: After deploying TriggerMesh, you can access the UI by navigating to the endpoint provided during the installation process. This endpoint is usually exposed via an ingress controller or a service in your Kubernetes cluster.
3. **Create and manage integrations**: Use the UI to create new event sources, define event targets, and apply transformations. The UI allows you to visually connect different components and configure their properties, making it easier to manage complex event-driven workflows.

For more details and step-by-step guides on using the TriggerMesh UI, you can visit their [documentation](https://docs.triggermesh.io).

**Next Steps:**
**a.** Explore the TriggerMesh UI to familiarize yourself with its capabilities and features.
**b.** Set up a sample integration using the UI to see how it simplifies the process.
