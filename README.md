# Pod broker web interface

This repo lets you build and create a custom app launcher portal for Selkies.

## Deployment

1. Build the custom web image with Cloud Build:

```bash
PROJECT_ID=$(gcloud config get-value project)
```

```bash
gcloud builds submit --project ${PROJECT_ID} -t gcr.io/${PROJECT_ID}/kube-pod-broker-custom-web:latest
```

2. Create a Secret Manager Secret to tell the core selkies repo not to use the image from the core selkies repo:

```bash
gcloud secrets create broker-web-image --replication-policy=automatic --data-file - <<EOF
gcr.io/${PROJECT_ID}/kube-pod-broker-custom-web:latest
EOF
```

3. From the Selkies core repo, run Cloud Build from the `setup/manifests` directory to deploy the updated web interface.

## Local development

Requirements:
- kubectl
- nodejs

1. Install node modules:

```bash
npm install
```

2. Start dev server with port-forward to `pod-broker-0` in current Kubernetes namespace:

```bash
./dev_server.sh
```

3. Open URL displayed in dev server output.