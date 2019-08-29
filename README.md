# km

Km is a thin wrapper for kubectl which helps with the EKS kubeconfig management.
Any other command will be proxied to kubectl so km command can be used in the same way as kubectl.

Check `km -h` for all the available options.

*Tested with EKS access via iam assume role and secured with MFA.*

## Requirements

- aws-iam-authenticator
- awscli 1.16.+ (configured like on the example)
- kubectl 1.15+
- yubikey (optional)
- gnu-sed, gnu-grep
- [fzf](https://github.com/junegunn/fzf)

## Installation

```
curl -L https://raw.githubusercontent.com/cststack/km/master/bin/km -o /usr/local/bin/km
chmod +x /usr/local/bin/km
```

## Features

- multiple EKS clusters can be configured. If they have the same name they can be assigned aliases.
- context change for the cluster and the namespace (fzf is required for this)
- run any kubectl commands via km
- if the [ykman](https://github.com/Yubico/yubikey-manager) is installed and an Yubikey is available on the system the mfa insert is seamless. If multiple profiles are available the ykman profile can be configured via MFA_ACCOUNT variable.
- the session validity can be configured with the SESSION_DURATION variable. By default set to 15 minutes (900 seconds).

## Examples

### Add new EKS cluster

With iam-role resolved from ~/.aws/credentials from the aws_profile
```
km add --region eu-west-1 --profile aws_profile --cluster-name aws-eks-eu-west-1 --cluster-alias eks-dev
```

With iam-role

```
km add --region eu-west-1 --profile aws_profile --cluster-name aws-eks-eu-west-1 --cluster-alias eks-dev --iam-role arn:aws:iam::111111111111:role/EksRole
```

### AWS credentials file

```
[default]
region = eu-west-1
aws_access_key_id = xxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
mfa_serial = arn:aws:iam::000000000000:mfa/AWS_USERNAME

[aws_profile]
role_arn = arn:aws:iam::111111111111:role/EksRole
region = eu-west-1
source_profile = default
mfa_serial = arn:aws:iam::000000000000:mfa/AWS_USERNAME
```

## Credits
- the guys who has created [kns](https://github.com/blendle/kns)
- [junegunn](https://github.com/junegunn) for the fzf tool
- and to the open source community
