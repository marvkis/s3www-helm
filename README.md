# s3www-helm
Just a helm chart to deploy s3www

It currently uses the s3www fork from https://github.com/anbraten/s3www / https://hub.docker.com/r/anbraten/s3www/tags as the 404.html feature is useful for me.

## TL;DR

Add the Helm repo
```bash

helm repo add marvkis-charts https://marvkis.github.io/charts
helm repo update

```

Create a values.yaml
```yaml
s3www:
  endpoint: http://minio.namespace.svc.cluster.local:9000
  bucket: www.example.org
  accessKeySecret: minio-www.example.org-secrets
  accessKeySecretKey: serviceKeyUser
  secretKeySecret: minio-www.example.org-secrets
  secretKeySecretKey: serviceKeySecret

ingress:
  enabled: true
  annotations:
    kubernetes.io/ingress.class: traefik
    traefik.ingress.kubernetes.io/router.entrypoints: web,websecure
  hosts:
  - host: www.example.org

```

Install using the `values.yaml`
```bash
helm install --values values.yaml s3www-example marvkis-charts/s3www
```

To show the generated passwords you can dump the secrets using this kubectl command:


## Uninstalling
```bash

helm delete s3www-example

```

## Parameters

| Parameter                      | Description                                     | Default |
| ------------------------- | ----------------------------------------------- | ----- |
| `s3www.endpoint` | S3 server url like (https://s3.exmaple.com) **required** |`""` |
| `s3www.bucket` | Bucket name which hosts static file **required** |`""` |
| `s3www.bucketPath` | Serve files from a sub folder of the bucket |`""` |
| `s3www.accessKey` | Access key of S3 storage |`""` |
| `s3www.accessKeySecret` | Get S3 Access key from a secret - this is the secret name |`""` |
| `s3www.accessKeySecretKey` | Get S3 Access key from a secret - this is the key within the name |`""` |
| `s3www.secretKey` | Secret key of S3 storage |`""` |
| `s3www.secretKeySecret` | Get S3 Secret key from a secret - this is the secret name |`""` |
| `s3www.secretKeySecretKey` | get S3 Secret key from a secret - this is the key within the name |`""` |
| `s3www.useCache` | Enable caching of http responses |`false` |
| `s3www.cacheTTL` | TTL of items in cache in seconds |`300` |
|  | Standard parameters |  |
| `replicaCount`    | How many replicas of Chisel to run                    | `1` |
| `image.repository` | The Docker image to run | jpillora/chisel  |
| `image.pullPolicy` | The image pull policy for the Chisel Docker image container | `IfNotPresent` |
| `image.tag` | The Docker tag (comes after the `:`). | Defaults to the chart `appVersion` |
| `image.customRegistry` | The pre-pended container registry for the image. For example: `gchr.io` | `` |
| `imagePullSecrets` | The image pull secrets field for the deployment | `` |
| `nameOverride` | Used to override the name | `` |
| `fullNameOverride` | Used to override the full name of the release | `` |
| `podAnnotations` | Used to add annotations to each pod deployed by this chart | `` |
| `podSecurityContext` | The pod security context will be used by the container runtime | `` |
| `securityContext` | The security context will be used by the container runtime | `` |
| `service.type` | The Kubernetes Service type field | `ClusterIP` |
| `service.port` | The port of the Kubernetes service that selects the pods running Chisel | `80` |
| `ingress.enabled` | Allows generation of Kubernetes Ingress object | `true` |
| `ingress.annotations` | Annotations added to the Ingress object | `` |
| `ingress.nginxRewrite` | Used when you require a context root for Chisel. For example `/chisel` | `false` |
| `ingress.nginxRegex` | When you want to apply regex matching for the Chisel path. | `false` |
| `ingress.tls.secretName` | The name of the Kubernetes secret to store the TLS secret for Ingress | `` |
| `resources.requests.cpu` | The requested CPU per container | `10m` |
| `resources.requests.memory` | The requested memory per container | `16Mi` |
| `resources.limits.cpu` | The CPU limit per container | `100m` |
| `resources.limits.memory` | The memory limit per container | `64Mi` |
| `resources` | The Kubernetes `resources` section for CPU and memory requests and memory | `` |
| `resources` | The Kubernetes `resources` section for CPU and memory requests and memory | `` |
| `nodeSelector` | See Kubernetes docs | `` |
| `tolerations` | See Kubernetes docs | `` |
| `affinity` | See Kubernetes docs | `` |