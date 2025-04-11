# IAM
simulator : https://policysim.aws.amazon.com/home/index.jsp?#roles/role-regis-test-iam-policies

An IAM policy can be attached to 
users : Granted access to EC2 / S3
group
rôle

#Liens

	§ https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html
	§ https://www.datadoghq.com/blog/iam-least-privilege/

	§ https://github.com/TryTryAgain/aws-iam-actions-list

	§ https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html


## User Exemples : 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "DynamoDB:*",
        "s3:*"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-number-without-hyphens:table/table-name",
        "arn:aws:s3:::bucket-name",
        "arn:aws:s3:::bucket-name/*"
      ]
    }
  ]
}

```
The NotResource element helps to ensure that users can’t use any other DynamoDB or S3 actions or resources except the actions and resources that are specified in the policy.
/!\ Even if permissions have been granted in an other policy, an explicit deny statement takes precedence over an allow statement
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "DynamoDB:*",
        "s3:*"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-number-without-hyphens:table/table-name",
        "arn:aws:s3:::bucket-name",
        "arn:aws:s3:::bucket-name/*"
      ]
    },
    {
      "Effect": "Deny",
      "Action": [
        "dynamodb:*",
        "s3:*"
      ],
      "NotResource": [
        "arn:aws:dynamodb:region:account-number-without-hyphens:table/table-name",
        "arn:aws:s3:::bucket-name",
        "arn:aws:s3:::bucket-name/*"
      ]
    }
  ]
}

```
# Identity based policies
## AWS managed policies
AWS managed policies are managed policies that are created and managed by AWS. If you are new to using policies, we recommend that you start by using AWS managed policies. IAM has a library of over 1,000 AWS managed policies.

## Customer managed policies
Customer-managed policies are managed policies that you create and manage in your AWS account. Customer managed policies provide more precise control over your policies than AWS managed policies. You can create and edit an IAM policy in the visual editor or by creating the JSON policy document directly.

## Inline policies
Inline policies are policies that you create and manage and that are embedded directly into a single user, group, or role. Using inline policies to grant permissions to users is high maintenance and not recommended.

# Resource based policies 
These policies specify who can access the resource and what actions they can perform on it.
That type of policy is defined on the resource itself instead of creating a separate IAM policy document to attach.

