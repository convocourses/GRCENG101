# Lab 2.3 evidence

After `terraform apply`, capture machine-readable compliance evidence here:

```bash
cd terraform/primitives/compliant-s3
terraform show -json tfplan > ../../../evidence/lab-2-3/plan.json
terraform show -json        > ../../../evidence/lab-2-3/state.json
```

- `plan.json` — the approved change set (pre-deploy evidence)
- `state.json` — deployed configuration: SC-28 (sse_algorithm=AES256), AC-3
  (four public-access-block flags), CM-6 (tags, versioning), AU-3/AU-6
  (logging target_bucket)
