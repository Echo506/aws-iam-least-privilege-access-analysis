# Evidence Screenshots

This folder contains sanitized screenshots collected during the AWS IAM least-privilege access-control lab.

All screenshots were reviewed to remove or obscure sensitive information, including account IDs, sign-in URLs, passwords, session information, public IP addresses, instance IDs, and other environment-specific identifiers.

## Evidence Categories

- IAM group creation
- EC2 read-only policy assignment
- IAM user creation and group membership
- Successful EC2 read-only access
- Denied EC2 termination attempt
- Default deny behavior
- RDS read-only access assignment
- Final lab validation

## Evidence and Validation

The screenshots below document the implementation and validation of least-privilege permissions. All images were sanitized before publication to remove credentials, account identifiers, session data, resource IDs, public IP addresses, and other sensitive details.

## Security Overview

![Security Overview](images/00-security-overview.png)

### 1. IAM Group Creation

The `SupportEngineers` IAM group was created to centrally manage permissions for users with the support-engineer role.

![SupportEngineers IAM group created](images/01-group-creation.png)

### 2. EC2 Read-Only Access

The AWS managed policy `AmazonEC2ReadOnlyAccess` was selected and attached to the `SupportEngineers` group.

![AmazonEC2ReadOnlyAccess selected](images/02-ec2-readonly-policy.png)

### 3. User and Group Membership

The IAM user `support-engineer-1` was created and assigned to the `SupportEngineers` group. This allows the user to inherit group permissions rather than receiving policies directly.

![User assigned to SupportEngineers group](images/04-user-group-membership.png)

### 4. EC2 Read-Only Validation

The support-engineer user could view EC2 instance information, confirming that the read-only policy provided the required operational visibility.

![EC2 read-only access](images/05-ec2-read-access.png)

### 5. Denied EC2 Termination

An attempt to terminate an EC2 instance was denied because no identity-based policy allowed the `ec2:TerminateInstances` action. This confirms that least-privilege restrictions were correctly enforced.

![EC2 termination request](images/06-termination-request.png)

![EC2 termination denied](images/07-termination-denied.png)

### 6. Default Deny Behavior

The restricted dashboard panels and access-denied notifications show that permissions not explicitly granted remain unavailable. This demonstrates AWS's default deny model.

![Default deny validation](images/08-default-deny.png)

### 7. Final Lab Validation

The AWS SimuLearn validation result confirmed successful completion of the activity, including the required read-only access configuration for the `SupportEngineers` group.

![AWS SimuLearn assignment completed](images/10-lab-validation-success.png)
