# Argo Rollouts and Helm

Argo Rollouts will respond to changes in Rollout resources
regardless of the event source. If you package your manifest
with the Helm package manager you can perform Progressive Delivery deployments with Helm

1. Install the Argo Rollouts controller in your cluster: https://github.com/argoproj/argo-rollouts#installation
2. Install the `helm` executable locally: https://helm.sh/docs/intro/install/

## Deploying the initial version

To deploy the first version of your application:

```
git clone https://github.com/Ankur-Rtc/argo-rollout-blue-green-deployment.git
cd argo-rollout-blue-green-deployment/
helm install example .
```

Your application will be deployed and exposed via the `example-helm-guestbook` service

## Perform the second deployment

To deploy the updated version using a Blue/Green strategy:

```
helm upgrade example .  --set image.tag=1.22
```

Now, two versions will exist in your cluster (and each one has an associated service)

```
kubectl-argo-rollouts get rollout example-helm-guestbook
```

## Promoting the rollout

To advance the rollout and make the new version stable

```
kubectl-argo-rollouts promote example-helm-guestbook
```

This promotes container image `nginx:latest` to `green` status and `Rollout` deletes old replica which runs `nginx:1.22`.
