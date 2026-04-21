# AWS Setup Instructions
You should have received an email with the subject "CS152 Lab 6 AWS Access." In that email, you will find your AWS Access Key, Secret Access Key, and a file attached for your SSH access.

## Initial Setup

### Install AWS CLI
If you do not have AWS CLI installed on your local computer, follow these instructions to set it up: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

Once you have installed AWS CLI, configure it with the credentials attached in the email.
```bash
aws configure --profile cs152
```

Enter the details from the email as follows:
```
AWS Access Key ID:     {access_key}
AWS Secret Access Key: {secret_key}
Default region:        {region}
Default output format: None
```

### Setup SSH
Save the attached SSH key to your ~/.ssh/ directory:
```bash
mv {Downloads/key_filename} ~/.ssh/
chmod 400 ~/.ssh/{key_filename}
```

Setup a SSH rule in your `~/.ssh/config` file:
```bash
Host trn1_cs152
    HostName {public_dns}
    User ubuntu
    IdentityFile ~/.ssh/{key_filename}
```

## Daily Usage
You will use AWS CLI to start and stop your instances.

To start your instance:
```bash
aws ec2 start-instances --instance-ids {instance_id} --profile cs152
```

After a few seconds, you can connect to it with SSH, using terminal or your favorite IDE.

To stop your instance:
```bash
aws ec2 stop-instances --instance-ids {instance_id} --profile cs152
```

> [!WARNING]
>
> Do not leave your AWS instance running! Make sure to turn off your instance when you are taking a break or when you are done working. The instance will automatically shutoff if it detects more than 45 minutes of idle usage.
