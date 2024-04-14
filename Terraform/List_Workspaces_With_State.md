# List Workspaces With State

Given `terraform.conf`:

```properties
bucket="{bucket}"
kms_key_id="kms_key_id_arn"
dynamodb_table="{dynamodb_table}"
```

Initialise the project

```shell
rm -fr .terraform
rm -f .terraform.lock.hcl
terraform init -backend-config=terraform.conf
```

Then, run the following: 

```
terraform workspace list \
  | sed 's/^* //' \
  | xargs -L 1 -I % sh -c 'terraform workspace select % && terraform workspace list | wc -l' sh \
  | xargs -L 2 \
  | sed -r 's/^.*Switched to workspace ([a-zA-Z0-9-]+).* ([0-9]+)$/\1 \2/'
```
