# AWS IAM Least-Privilege Access Analysis

## Overview

This project documents a hands-on AWS Identity and Access Management (IAM) security scenario focused on least-privilege access control for support engineers.

The goal is to manage permissions through an IAM group instead of assigning policies directly to individual users. The solution uses AWS managed policies to provide controlled, read-only access to Amazon EC2 and Amazon RDS resources.

## Solution Request

Implement IAM groups with least-privilege permissions to control support engineers' access to AWS resources.

## Scenario

An organization needs to provide its support engineers with visibility into AWS resources without granting excessive administrative permissions.

To achieve this, the organization creates an IAM group named `SupportEngineers`. Users assigned to this group inherit permissions through group membership. This approach improves consistency, simplifies user administration, and supports the principle of least privilege.

The project begins with Amazon EC2 read-only access. Later, it expands access to Amazon RDS after a business need is identified and approved.

## Learning Objectives

- Create and manage IAM groups.
- Create IAM users and assign them to groups.
- Apply AWS managed policies to IAM groups.
- Implement least-privilege access control.
- Validate allowed and denied actions.
- Understand AWS default deny behavior.
- Expand permissions in a controlled manner.
- Document cloud security configurations and validation results.

## AWS Services Used

- AWS Identity and Access Management (IAM)
- Amazon Elastic Compute Cloud (Amazon EC2)
- Amazon Relational Database Service (Amazon RDS)
- AWS managed IAM policies

## IAM Components

| Component | Value | Purpose |
|---|---|---|
| IAM group | `SupportEngineers` | Central permission container for support engineers |
| IAM user | `support-engineer-1` | Example support engineer account |
| EC2 policy | `AmazonEC2ReadOnlyAccess` | Allows read-only visibility into EC2 resources |
| RDS policy | `AmazonRDSReadOnlyAccess` | Allows read-only visibility into RDS resources |

## Access-Control Design

### Initial access model

```text
support-engineer-1
        |
        v
SupportEngineers
        |
        v
AmazonEC2ReadOnlyAccess
        |
        v
View EC2 resource information
```

### Expanded access model

```text
support-engineer-1
        |
        v
SupportEngineers
        |
        +------------------------------+
        |                              |
        v                              v
AmazonEC2ReadOnlyAccess      AmazonRDSReadOnlyAccess
        |                              |
        v                              v
View EC2 information          View RDS information
```

## Implementation Steps

### 1. Create the IAM group

Create an IAM group named `SupportEngineers`.

This group centralizes permissions for users who perform support-related duties. Instead of configuring policies for each user individually, permissions are attached once to the group.

### 2. Attach EC2 read-only permissions

Attach the AWS managed policy:

```text
AmazonEC2ReadOnlyAccess
```

This policy allows members of the group to view Amazon EC2 resources, instance details, and related configuration information.

The policy does not allow destructive actions such as terminating EC2 instances.

### 3. Create the IAM user

Create an IAM user named:

```text
support-engineer-1
```

This user represents a support engineer who needs controlled operational access.

### 4. Add the user to the group

Add `support-engineer-1` to the `SupportEngineers` group.

The user automatically inherits the permissions attached to the group.

### 5. Validate EC2 read access

Confirm that `support-engineer-1` can perform read-only actions, such as viewing EC2 instance information.

Expected result:

```text
EC2 instance information is visible.
```

### 6. Validate denied actions

Attempt a restricted action, such as terminating an EC2 instance.

Expected result:

```text
The request is denied because the user only has read-only permissions.
```

This is a positive security outcome because it proves the least-privilege configuration is working correctly.

### 7. Observe default deny for RDS

Before assigning RDS permissions, attempt to access Amazon RDS resources.

Expected result:

```text
Access is denied.
```

This demonstrates AWS's default deny model. A user cannot access a service unless an IAM policy explicitly allows it.

### 8. Grant RDS read-only access

Attach the following AWS managed policy to the `SupportEngineers` group:

```text
AmazonRDSReadOnlyAccess
```

This allows the group members to view Amazon RDS resources without allowing modifications, deletions, or database administration actions.

### 9. Complete the DIY validation

The final objective is to confirm that the `SupportEngineers` group has both required read-only policies:

```text
AmazonEC2ReadOnlyAccess
AmazonRDSReadOnlyAccess
```

The final validation should confirm that the group has read-only access to both Amazon EC2 and Amazon RDS.

## Security Concepts Demonstrated

### Least Privilege

Users receive only the permissions required to complete their job responsibilities. Support engineers can view resource information but cannot make unauthorized or destructive changes.

### Group-Based Access Control

IAM groups make permission management more efficient. New support engineers can receive the correct access by being added to the `SupportEngineers` group.

### Permission Inheritance

Users inherit permissions from their IAM group. This reduces administrative effort and helps maintain consistent access controls.

### AWS Managed Policies

AWS managed policies provide predefined permission sets. In this project, read-only AWS managed policies are used to limit operational risk.

### Default Deny

AWS denies all access unless an IAM policy explicitly allows an action. This behavior protects AWS resources from unauthorized access.

### Controlled Permission Expansion

Amazon RDS access is added only after a business requirement is identified. This demonstrates that permissions should be expanded gradually and intentionally.

## Validation Results

| Test | Expected result | Security meaning |
|---|---|---|
| View EC2 information | Allowed | Support engineer has required read-only access |
| Terminate EC2 instance | Denied | Least privilege prevents destructive actions |
| View RDS before policy assignment | Denied | AWS default deny is enforced |
| View RDS after policy assignment | Allowed | Controlled permission expansion works |
| Group policy validation | EC2 and RDS read-only policies assigned | Final configuration meets requirements |

## Evidence

Add screenshots to an `images/` folder using this naming convention:

```text
images/
├── 01-supportengineers-group.png
├── 02-ec2-readonly-policy.png
├── 03-support-engineer-user.png
├── 04-group-membership.png
├── 05-ec2-read-access.png
├── 06-ec2-termination-denied.png
├── 07-rds-access-denied.png
├── 08-rds-readonly-policy.png
└── 09-final-validation.png
```

Suggested evidence includes:

- IAM group creation page.
- `AmazonEC2ReadOnlyAccess` attached to the group.
- IAM user creation.
- User membership in `SupportEngineers`.
- Successful EC2 read-only access.
- Denied instance termination attempt.
- RDS access denial before RDS permissions are added.
- `AmazonRDSReadOnlyAccess` attached to the group.
- Successful final validation.

## Key Lessons Learned

- IAM groups provide scalable permission management for users with similar responsibilities.
- Least privilege reduces the risk of accidental or malicious changes.
- Denied actions can confirm that security restrictions are functioning correctly.
- AWS default deny protects resources until access is explicitly granted.
- AWS managed policies simplify controlled access assignment.
- Permission changes should be documented, justified, and validated.

## Project Structure

```text
aws-iam-least-privilege-access-analysis/
├── README.md
├── docs/
│   ├── iam-group-setup.md
│   ├── ec2-readonly-access.md
│   ├── user-permission-inheritance.md
│   ├── access-validation.md
│   ├── default-deny-rds.md
│   └── lessons-learned.md
├── diagrams/
│   └── iam-access-flow.md
└── images/
    └── lab-screenshots/
```

## Skills Demonstrated

- AWS IAM administration
- IAM users and groups
- AWS managed policies
- Least-privilege implementation
- Access-control validation
- EC2 and RDS permissions
- Cloud security documentation
- Technical reporting

## Author

**Wilfrido Pérez Romero**  
Cloud Security | IAM | GRC | Cybersecurity Portfolio

---

> This repository is an independent educational and portfolio project based on hands-on AWS IAM learning objectives. It does not reproduce AWS training material and is not an official AWS project.
