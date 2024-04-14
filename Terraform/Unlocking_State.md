# Unlocking Terraform State

Given `terraform.conf`:

```properties
bucket="{bucket}"
kms_key_id="{kms_key_id_arn}"
dynamodb_table="{dynamodb_table}"
```

Given error acquiring state lock:

```text
Lock Info:
  ID:        {lock-id}
  Path:      {bucket}/env:/{workspace}/{project}.tfstate
  Operation: OperationTypeInvalid
  ...
```

Run the following:

```shell
rm -fr .terraform
rm -f .terraform.lock.hcl
terraform init -backend-config=terraform.conf
terraform workspace select {workspace}
terraform force-unlock -force {lock-id}
```
