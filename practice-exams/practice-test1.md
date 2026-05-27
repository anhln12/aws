Question 1:

An enterprise runs a microservices-based application on Amazon EKS, deployed on EC2 worker nodes. The application includeds a frontend UI service that interacts with Amazone DynamoDB and a data-processing service that stores and rerieves files from Amazon S3. The organization needs to strictly enforce least privilege access: The UI Pods must access only DynamoDB, and the data-processing Pods must access only S3.

Which solution will best enforce these access controls within the EKS cluster?

A. Create IAM policies for DynamoDB and S3 access, and attach both the EC2 instance profile used by the EKS nodes. Use Kubernetes role-based access control (RBAC) to control service-level permissions within the cluster

B. Create separate Kubernetes service accounts for the UI and data services. Use IAM Roles for Service Accounts (IRSA) to map each service account to an IAM role with only the required permissions. Assign DynamoDB access to the UI Pods and S3 access to the data Pods.

C. Attach an IAM policy directly to each Pod using Kubernetes annotations. Assign the S3 policy to data-service Pods and the DynamoDB policy to UI Pods

D. Create one Kubernetes service account shared across all Pods. Attach a single IAM role to this account with both AmazonS3FullAccess and AmazonDynamoDBFullAccess policies
