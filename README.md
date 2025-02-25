# Kubernetes Domain Setup

This repository contains Kubernetes manifests for setting up a domain with TLS certificates using cert-manager and Ingress resources.

## Overview

These manifests handle the complete setup for deploying an application with proper domain configuration, TLS certificates, and external access through Ingress.

## Components

- **Deployment**: Application deployment configuration
- **Service**: Exposes the application within the cluster
- **Ingress**: Routes external traffic to your service (with TLS support)
- **ClusterIssuer**: cert-manager resource that handles certificate issuance
- **Certificate**: Defines the TLS certificate for your domain

## Prerequisites

- Kubernetes cluster
- kubectl configured to communicate with your cluster
- cert-manager installed on the cluster
- Domain name with DNS configured to point to your cluster's ingress controller

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/ManojKumawat90/k8s-domainsetup.git
   cd k8s-domainsetup
   ```

2. Modify the manifests to use your domain and application details:
   - Update domain names in `ingress.yaml` and `certificate.yaml`
   - Adjust the application details in `deployment.yaml` and `service.yaml`
   - Configure the email in `clusterissuer.yaml` for Let's Encrypt notifications

3. Apply the manifests:
   ```bash
   # Create the ClusterIssuer first
   kubectl apply -f clusterissuer.yaml
   
   # Apply the deployment and service
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   
   # Apply the certificate
   kubectl apply -f certificate.yaml
   
   # Finally, apply the ingress
   kubectl apply -f ingress.yaml
   ```

## Configuration Options

### TLS vs Non-TLS

- For HTTPS (recommended): Use `ingress.yaml`
- For HTTP only: Use `ingress.yaml--without-443`

### Certificate Issuer

The default setup uses Let's Encrypt with HTTP01 challenge. To modify this:
- Edit `clusterissuer.yaml` to change the certificate issuer configuration
- Adjust the corresponding references in `certificate.yaml` and `ingress.yaml`

## Verification

1. Check if the certificate is issued:
   ```bash
   kubectl get certificates
   ```

2. Verify the ingress is properly configured:
   ```bash
   kubectl get ingress
   ```

3. Test domain access:
   ```bash
   curl -L https://your-domain.com
   ```

## Troubleshooting

- **Certificate not issued**: Check cert-manager logs and ensure the ClusterIssuer is properly configured
- **Ingress not working**: Verify your domain DNS settings and ingress controller configuration
- **Application not accessible**: Check the Service and Deployment status with `kubectl get pods` and `kubectl get svc`

## License

[Insert your license information here]
