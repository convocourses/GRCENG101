# compliant-s3

This module enforces SC-28, AU-3, AU-6, CM-6, and AC-3 (NIST 800-53) on a
single S3 bucket. A primary data bucket gets AES-256 server-side encryption
(SC-28), versioning (CM-6), a full four-flag public access block (AC-3), and
the four required compliance tags via provider `default_tags` (CM-6). A
companion log bucket — encrypted and public-blocked to the same baseline —
receives S3 server access logs from the primary (AU-3 / AU-6). Compliance
evidence is captured as machine-readable JSON via `terraform show -json`
rather than screenshots.

## Usage

```bash
terraform init
terraform validate
terraform plan -out=tfplan
terraform apply -auto-approve tfplan

# Capture evidence (Lab 2.3)
mkdir -p ../../../evidence/lab-2-3
terraform show -json tfplan > ../../../evidence/lab-2-3/plan.json
terraform show -json        > ../../../evidence/lab-2-3/state.json
```

## Verification

```bash
BUCKET=$(terraform output -raw bucket_name)
aws s3api get-bucket-encryption   --bucket "$BUCKET" --region us-east-1
aws s3api get-bucket-versioning   --bucket "$BUCKET" --region us-east-1
aws s3api get-public-access-block --bucket "$BUCKET" --region us-east-1
```
