# Huly Helm Chart

This repository contains a Helm chart for deploying Huly in a Kubernetes cluster.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- Ingress controller (e.g., nginx-ingress)
- PersistentVolume provider

## Installation

```bash
# Add repository
helm repo add huly https://github.com/DreemiX/huly-chart/releases/download
helm repo update

# Install chart
helm install huly huly/huly-chart
```

Or install from local clone:

```bash
git clone https://github.com/DreemiX/huly-chart.git
cd huly-chart
helm install huly .
```

## Configuration

### Main Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `global.hulyVersion` | Huly version | `v0.6.377` |
| `config.hostAddress` | Main domain for Huly | `localhost` |
| `config.secure` | Use HTTPS | `false` |
| `config.title` | Instance name | `Huly` |

### Services Configuration

Each service can be configured separately:

```yaml
services:
  front:
    enabled: true
    replicas: 1
    resources:
      limits:
        memory: "512Mi"
        cpu: "500m"
  
  account:
    enabled: true
    replicas: 1
    resources:
      limits:
        memory: "512Mi"
```

### Ingress Configuration

```yaml
ingress:
  enabled: true
  className: nginx
  annotations: {}
  hosts:
    - host: huly.example.com
      paths:
        - path: /
          pathType: Prefix
```

### Database Configuration

```yaml
mongodb:
  enabled: true
  persistence:
    size: 10Gi
```

### Storage Configuration

```yaml
minio:
  enabled: true
  persistence:
    size: 10Gi
```

## Configuration Examples

### Minimal Configuration

```yaml
# values.yaml
config:
  hostAddress: "huly.example.com"
```

### Production Configuration

```yaml
# values.yaml
config:
  hostAddress: "huly.example.com"
  secure: true

ingress:
  enabled: true
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod

persistence:
  storageClass: "standard"

resources:
  requests:
    memory: "512Mi"
    cpu: "250m"
  limits:
    memory: "1Gi"
    cpu: "500m"
```

## Upgrading

To upgrade Huly to a new version:

```bash
helm repo update
helm upgrade huly huly/huly-chart
```

## Uninstalling

To remove Huly:

```bash
helm uninstall huly
```

## Architecture

The Helm chart installs the following components:

- Frontend service
- Account service
- Transactor service
- Collaborator service
- Fulltext service
- Stats service
- MongoDB
- MinIO
- Elasticsearch

## Troubleshooting

### Check Pod Status

```bash
kubectl get pods -l app.kubernetes.io/instance=huly
```

### View Logs

```bash
# Frontend logs
kubectl logs -l app.kubernetes.io/name=front

# Specific pod logs
kubectl logs <pod-name>
```

### Common Issues

1. **Pods in Pending State**
   - Check PVC status
   - Verify storage class availability
   - Check node resources

2. **Service Connectivity Issues**
   - Verify service DNS resolution
   - Check ingress configuration
   - Validate service endpoints

3. **Database Connection Issues**
   - Check MongoDB pod status
   - Verify connection strings
   - Check service discovery

## Development

### Testing the Chart

```bash
# Lint the chart
helm lint .

# Test the chart installation
helm install huly . --dry-run --debug

# Test template rendering
helm template huly .
```

### Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## Support

If you encounter any problems or have questions:

1. Check existing [issues](https://github.com/DreemiX/huly-chart/issues)
2. Create a new issue with a detailed description

## License

MIT License

## References

- [Huly Documentation](https://github.com/hcengineering/huly-selfhost)
- [Helm Documentation](https://helm.sh/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)

## Maintainers

- [@DreemiX](https://github.com/DreemiX)