# AWS IAM Security Audit

## Project Overview

I conducted an AWS Identity and Access Management (IAM) security audit in a personal AWS Free Tier environment. The objective was to identify common identity and access-management weaknesses, assess their potential security impact, apply appropriate remediation, and verify the changes.

I created a controlled environment with five test IAM users and deliberately introduced security weaknesses. I reviewed their permissions and credentials, identified excessive access and unnecessary identities, and applied corrective actions based on the principle of least privilege.

## Objectives

- Identify IAM users with excessive permissions.
- Investigate wildcard permissions and their potential security impact.
- Identify unnecessary or unused access keys.
- Review dormant or unnecessary IAM identities.
- Apply least-privilege permissions.
- Verify remediation using AWS IAM tools.

## Environment and Tools

- **Cloud platform:** Amazon Web Services (AWS)
- **Service:** AWS Identity and Access Management (IAM)
- **Storage service:** Amazon Simple Storage Service (Amazon S3)
- **Testing tool:** IAM Policy Simulator
- **Documentation:** GitHub
- **Policy format:** JSON

## Audit Findings and Remediation

### F1 - dev-intern: Excessive Administrative Permissions

**Finding:** The `dev-intern` user had the AWS-managed `AdministratorAccess` policy attached.

This policy provided broad permissions, including administrative IAM actions that were unnecessary for the user's intended role.

**Risk:** Excessive permissions could allow an identity with limited development responsibilities to perform sensitive administrative operations, such as creating IAM users, creating access keys, and modifying user policies.

**Remediation:** I removed `AdministratorAccess` and attached a custom, restricted S3 policy named `dev-intern-limited`.

**Verification:** I used IAM Policy Simulator to test the replacement permissions. The intended S3 actions, including `s3:ListBucket` and `s3:GetObject`, were allowed, while other tested actions were denied.

**Evidence:**

| Screenshot | Description |
|---|---|
| [F1-01](screenshots/F1-01-dev-intern-access-advisor.png.png) | Access Advisor |
| [F1-02](screenshots/F1-02-dev-intern-attached-policy.png.png) | Original attached policy |
| [F1-03](screenshots/F1-03-dev-intern-policy-details.png.png) | AdministratorAccess policy details |
| [F1-04](screenshots/F1-04-dev-intern-privilege-tests.png.png) | Administrative privilege tests |
| [F1-05](screenshots/F1-05-dev-intern-remediation.png.png) | Removal of excessive permissions |
| [F1-06](screenshots/F1-06-dev-intern-limited-policy.png.png) | Restricted S3 policy details |
| [F1-07](screenshots/F1-07-dev-intern-limited-policy-attached.png.png) | Replacement policy attached |
| [F1-08](screenshots/F1-08-dev-intern-s3-policy-simulator-results.png.png) | Policy Simulator results |

### F2 - ops-wildcard: Overly Broad Wildcard Permissions

**Finding:** The `ops-wildcard` user had a policy containing overly broad wildcard permissions.

**Risk:** Wildcard permissions can grant access to more AWS actions and resources than an identity requires. This increases the potential impact of compromised credentials or misuse.

**Remediation:** I replaced the original wildcard policy with a more restrictive policy named `ops-limited`.

The original and replacement policy documents are available in the `policies/` directory:

- [before-wildcard-policy.json](policies/before-wildcard-policy.json) - Original policy.
- [after-wildcard-policy.json](policies/after-wildcard-policy.json) - Replacement policy.

**Verification:** I used IAM Policy Simulator to test the replacement policy. Of the actions tested, only `s3:ListBucket` was allowed.

**Evidence:**

| Screenshot | Description |
|---|---|
| [F2-01](screenshots/F2-01-ops-wildcard-access-advisor.png.png) | Access Advisor |
| [F2-02](screenshots/F2-02-ops-wildcard-attached-policy.png.png) | Original attached policy |
| [F2-03](screenshots/F2-03-ops-wildcard-policy-details.png.png) | Original wildcard policy details |
| [F2-04](screenshots/F2-04-ops-wildcard-policy-remediation.png.png) | Policy remediation |
| [F2-05](screenshots/F2-05-ops-wildcard-limited-policy-details.png.png) | Replacement policy details |
| [F2-06](screenshots/F2-06-ops-wildcard-limited-policy-attached.png.png) | Replacement policy attached |
| [F2-07](screenshots/F2-07-ops-wildcard-policy-simulator-results.png.png) | Policy Simulator results |

### F3 - old-contractor: Multiple Active Access Keys

**Finding:** The `old-contractor` user had two active access keys.

**Risk:** Retaining unnecessary active access keys increases the number of credentials that could potentially be exposed, stolen, or misused.

**Remediation:** I deactivated and deleted both access keys.

**Verification:** I reviewed the access-key status and subsequent removal.

**Evidence:**

| Screenshot | Description |
|---|---|
| [F3-01](screenshots/F3-01-old-contractor-security-credentials.png.png) | Security credentials |
| [F3-02](screenshots/F3-02-old-contractor-two-active-keys.png.png) | Two active access keys |
| [F3-03](screenshots/F3-03-old-contractor-keys-deactivated.png.png) | Access keys deactivated |
| [F3-04](screenshots/F3-04-old-contractor-keys-deleted.png.png) | Access keys deleted |

### F4 - legacy-app: Unnecessary Active Access Key

**Finding:** The `legacy-app` user had one active access key.

**Risk:** An unnecessary active credential creates an additional opportunity for unauthorised access if it is exposed or compromised.

**Remediation:** I deactivated and deleted the access key.

**Verification:** I reviewed the access-key status and its removal.

**Evidence:**

| Screenshot | Description |
|---|---|
| [F4-01](screenshots/F4-01-legacy-app-active-access-key.png.png) | Active access key |
| [F4-02](screenshots/F4-02-legacy-app-key-deactivated.png.png) | Access key deactivated |
| [F4-03](screenshots/F4-03-legacy-app-key-deleted.png.png) | Access key deleted |

### F5 - unused-user: Unnecessary IAM Identity

**Finding:** An unnecessary IAM user named `unused-user` existed in the environment.

**Risk:** Unnecessary IAM identities increase the number of accounts that must be monitored and secured.

**Remediation:** I deleted the unused IAM user.

**Evidence:**

| Screenshot | Description |
|---|---|
| [F5-01](screenshots/F5-01-unused-user-account.png.png) | Original IAM user account |
| [F5-02](screenshots/F5-02-unused-user-details.png.png) | User details |
| [F5-03](screenshots/F5-03-unused-user-deletion.png.png) | User deletion |

## Verification Approach

I used AWS IAM tools to review and verify the changes made during the audit.

Verification activities included:

- Reviewing attached IAM policies.
- Inspecting policy permissions and resource scopes.
- Using IAM Policy Simulator to test selected allowed and denied actions.
- Reviewing access-key statuses and removal.
- Confirming the removal of the unnecessary IAM user.


## Project Scope and Limitations

This was a controlled IAM security exercise conducted in a personal AWS account using test identities.

AWS Organizations Service Control Policies (SCPs) were not configured or used as part of this audit.

The project focused on IAM identity management, least-privilege policy design, resource-level permission scoping, access-key hygiene, and the removal of unnecessary identities.

## Key Learnings

Through this project, I gained practical experience with:

- Reviewing IAM users and attached permissions.
- Recognising excessive administrative permissions.
- Understanding the security implications of wildcard policies.
- Writing and reviewing JSON-based IAM policies.
- Applying least-privilege access to S3 resources.
- Using IAM Policy Simulator to evaluate permissions.
- Managing and removing unnecessary access keys.
- Identifying and removing unnecessary IAM identities.
- Documenting security findings, remediation, and verification evidence.

## Repository Structure

```text
aws-iam-security-audit/
│
├── policies/
│   ├── before-wildcard-policy.json
│   └── after-wildcard-policy.json
│
├── screenshots/
│   ├── F1-01 to F1-08 - dev-intern
│   ├── F2-01 to F2-07 - ops-wildcard
│   ├── F3-01 to F3-04 - old-contractor
│   ├── F4-01 to F4-03 - legacy-app
│   └── F5-01 to F5-03 - unused-user
│
└── README.md
```

## Conclusion

This project gave me practical experience investigating and improving AWS IAM security. It reinforced the importance of granting identities only the permissions required for their responsibilities, regularly reviewing access credentials, and removing identities that are no longer needed.

It also demonstrated the importance of testing permission changes rather than assuming that a policy is secure simply because it has been updated.
