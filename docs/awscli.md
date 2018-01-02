# Setting up the AWS CLI

Official AWS documentation: https://aws.amazon.com/documentation/cli/

## Installation

`pip` is one possible method: `pip install awscli`.

See more: https://docs.aws.amazon.com/cli/latest/userguide/installing.html

## Configure

Set your default credentials by running `aws configure`.

To save your connection credentials for multiple AWS accounts, use named profiles (https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html).

Edit `~/.aws/credentials`:

```ini
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[user2]
aws_access_key_id=AKIAI44QH8DHBEXAMPLE
aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```


To use the named profile, add the `--profile` command to your command:

```
$ aws ec2 describe-instances --profile user2
```

or

```
$ aws s3 sync public s3://bucket-name --delete --profile user2
```
