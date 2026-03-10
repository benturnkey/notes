# AWS Cloud Control Manager and Node Termination Handler

| Feature                 | AWS Cloud Controller (CCM)                                                          | Node Termination Handler (NTH)                                          |
| :---------------------- | :---------------------------------------------------------------------------------- | :---------------------------------------------------------------------- |
| **Primary Job**         | Infrastructure Bridge: Manages the lifecycle of AWS resources (Nodes, LBs, Routes). | Preparation: Drains/evicts pods before the EC2 instance is gone.        |
| **Node Initialization** | Annotates nodes with EC2 metadata (InstanceID, Region, Zone) and IPs.               | N/A                                                                     |
| **Node Health/Cleanup** | Post-Termination: Deletes the K8s Node object after the EC2 instance is gone.       | Pre-Termination: Handles graceful eviction before the instance is gone. |
| **Service Integration** | Configures AWS Load Balancers (Classic/NLB) for Kubernetes Services.                | N/A                                                                     |
| **Networking**          | Manages VPC Routing tables for pod-to-pod communication in non-overlay networks.    | N/A                                                                     |
| **Event Source**        | Periodically polls the AWS EC2 API for state changes.                               | Listens to IMDS or SQS for interruption notices.                        |
| **Use Case**            | Essential for keeping the cluster state in sync with AWS reality.                   | Critical for Spot Instances and proactive maintenance.                  |
| **Action**              | Synchronizes cloud state to the API server (Labels, IPs, Deletion).                 | Executes Cordon/Drain to move pods safely.                              |
