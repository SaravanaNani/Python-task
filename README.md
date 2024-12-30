### README: SSH Key-Based Authentication for GCP VM Access
Overview
This document provides step-by-step instructions to set up SSH key-based authentication for connecting to a GCP VM. Two approaches are covered:

### 1. Static Approach: Manually adding SSH keys to a specific VM.
Dynamic Approach: Using GCP's OS Login feature for centralized SSH key management.

Prerequisites

Google Cloud SDK: Ensure the gcloud command-line tool is installed and authenticated. Installation Guide

SSH Client: Available by default on Linux and macOS. Windows users can use tools like PuTTY or enable OpenSSH.

### 1. Generating SSH Key Pair

Begin by generating an SSH key pair on your local machine.

```ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```
-t rsa: Specifies the RSA algorithm.
-b 4096: Sets the key length to 4096 bits.
-C "your_email@example.com": Adds a label for the key.When prompted:

File Location: Press Enter to accept the default (~/.ssh/id_rsa) or specify a custom path.

Passphrase: Optionally, set a passphrase for added security.

### 2. Static Approach: Manually Adding SSH Key to a VM

# 2.1. Using the GCP Console

Access VM Instances: Navigate to the Google Cloud Console.

Go to Compute Engine -> VM Instances.

Edit SSH Keys:

Click on the desired VM's name. -> Scroll to the SSH Keys section and click Edit.

Add Public Key:

Retrieve your public key:
```
cat ~/.ssh/id_rsa.pub
```
Copy and paste its content into the SSH Keys field.

Save Changes:

-> Click Save to apply the changes.

# 2.2. Using the gcloud Command

Add SSH Key:

```
gcloud compute instances add-metadata INSTANCE_NAME \
--metadata ssh-keys="USERNAME:$(cat ~/.ssh/id_rsa.pub)"
```
Replace:

INSTANCE_NAME: Your VM's name.
USERNAME: Desired SSH username.

### 3. Dynamic Approach: Using OS Login

GCP's OS Login allows centralized SSH key management via IAM roles.

#3.1. Enable OS Login

Set Project-Wide Metadata:

```
gcloud compute project-info add-metadata --metadata enable-oslogin=TRUE
```
Set Instance Metadata (if needed):

```
gcloud compute instances add-metadata INSTANCE_NAME --metadata enable-oslogin=TRUE
```
# 3.2. Assign IAM Roles

Grant OS Login Role:

```
gcloud projects add-iam-policy-binding PROJECT_ID \
--member="user:your_email@example.com" \
--role="roles/compute.osLogin"
```
Replace PROJECT_ID with your GCP project ID.

For Admin Access (optional):

```
gcloud projects add-iam-policy-binding PROJECT_ID \
--member="user:your_email@example.com" \
--role="roles/compute.osAdminLogin"
```
3.3. Upload SSH Keys
Add SSH Key to Your Google Account:

```
gcloud compute os-login ssh-keys add --key-file=~/.ssh/id_rsa.pub
```
### 4. Connecting to the GCP VM

#4.1. Using the gcloud Command

```
gcloud compute ssh USERNAME@INSTANCE_NAME --zone ZONE
```
Replace:

USERNAME: Your SSH username.
INSTANCE_NAME: Your VM's name.
ZONE: The VM's zone.

# 4.2. Using the SSH Command

```
ssh USERNAME@EXTERNAL_IP
```
Replace:

USERNAME: Your SSH username.
EXTERNAL_IP: The VM's external IP address.

### 5. Verifying the Connection
Once connected, execute a simple command to verify:

```ls```
A successful response indicates a proper connection.

#### Security Considerations

Private Key Protection: Secure your private key (~/.ssh/id_rsa) with appropriate permissions.
```
chmod 600 ~/.ssh/id_rsa
```
Passphrase Usage: Enhance security by setting a passphrase during key generation.

References
Google Cloud: Connecting to Instances
Google Cloud: OS Login
