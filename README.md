# km

Km is a thin wrapper for kubectl which helps with the EKS kubeconfig management.
Any other command will be proxied to kubectl so km command can be used in the same way as kubectl.

Check `km -h` for all the available options.

*Tested with EKS access via iam assume role with/without MFA enabled.*

## Breaking changes from v0.0.1-beta4 to v0.0.1-beta5

- the clusters secured with MFA will need to be re-added using the `--mfa-profile` flag (check the examples below). This change will allow the management of the clusters without MFA including non AWS EKS cluster.

## Demo
![Demo](examples/demo.gif)

## Requirements

- aws-iam-authenticator
- jq
- awscli >= 1.16 (configured like on the example)
- kubectl >= 1.15
- yubikey (optional)
- gnu-sed, gnu-grep (MacOS)
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
- if [ykman](https://github.com/Yubico/yubikey-manager) is installed and the Yubikey is plugged there is no need to input the MFA token at all. Make sure the `--mfa-profile` matches the profile used for authentication in ykman.

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

With iam-role resolved from ~/.aws/credentials from the aws_profile and MFA
```
km add --region eu-west-1 --profile aws_profile --cluster-name aws-eks-eu-west-1 --cluster-alias eks-dev --mfa-profile mfaProfileName/myAwsSsoProfile
```

With iam-role and MFA

```
km add --region eu-west-1 --profile aws_profile --cluster-name aws-eks-eu-west-1 --cluster-alias eks-dev --iam-role arn:aws:iam::111111111111:role/EksRole  --mfa-profile mfaProfileName/myAwsSsoProfile
```

Import a KUBECONFIG file.
```
km --import-kubeconfig pathToKubeConfigFile
```

*The usual kubectl commands should be used to clear any cluster config from kubeconfig.*

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
or

```
[myAwsSsoProfile]
region = eu-west-1
aws_access_key_id = xxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
mfa_serial = arn:aws:iam::000000000000:mfa/AWS_USERNAME

[aws_profile]
role_arn = arn:aws:iam::111111111111:role/EksRole
region = eu-west-1
source_profile = myAwsSsoProfile
mfa_serial = arn:aws:iam::000000000000:mfa/AWS_USERNAME
```


## Credits
- the guys who has created [kns](https://github.com/blendle/kns)
- [junegunn](https://github.com/junegunn) for the fzf tool
- and to the open source community
