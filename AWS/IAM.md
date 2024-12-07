# IAM

An IAM policy can be attached to 
users : Granted access to EC2 / S3
group
rôle

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
