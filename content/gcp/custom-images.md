---
draft: false
title: Custom images
categories:
  - Gcp
---

## Make custom image publicly accessible

```bash
gcloud compute images add-iam-policy-binding image-name \
    --member='allAuthenticatedUsers' \
    --role='roles/compute.imageUser'
```
