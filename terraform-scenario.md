1. Scenario: Managing Multiple Environments

Question: How would you manage multiple environments (e.g., dev, staging, prod) with Terraform, ensuring that each environment has isolated resources and state? 
Answer: Use separate workspaces or directories for each environment. Store the state files in different locations (e.g., separate S3 buckets) and use terraform workspace select <env> to switch between environments. Alternatively, use a backend block with dynamic configuration to point to different state files.
