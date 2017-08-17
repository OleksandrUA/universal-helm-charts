# Universal Helm Charts used in Kubernetes

## Intro
* Let's assume that we have 4 stacks: local (minikube), development, staging and production
* Each stack has a Kubernetes namespace called local or development or staging or production
* kubectl must be installed
* helm must be installed

## Managing configs
```bash
export STACK_NAME=development

helm upgrade --install --namespace=${STACK_NAME} chart-config --set kubernetesNamespace=${STACK_NAME} -f ./chart-config/${STACK_NAME}/values.yaml ./chart-config/_common
```

## Managing secrets
```bash
# Do not forget that Secrets should be base64 encoded
export STACK_NAME=development

helm upgrade --install --namespace=${STACK_NAME} chart-secrets --set kubernetesNamespace=${STACK_NAME} -f ./chart-secrets/${STACK_NAME}/values.yaml ./chart-secrets/_common
```

## Managing/deploying services + working with cron-jobs
```bash
# Kubernetes context to use
export KUBERNETES_CONTEXT=development.example.com

# Kubernetes namespace to use
export HELM_NAMESPACE_NAME=development

# Amount of pods in ReplicaSet
export DESIRED_COUNT=2

# Docker image tag to deploy
export DOCKER_IMAGE_TAG=release1

# Service to deploy
export SERVICE_NAME=service1

# Selecting the proper Kubernetes context
kubectl config use-context ${KUBERNETES_CONTEXT}

# Deploying
helm upgrade --kube-context=${KUBERNETES_CONTEXT} --recreate-pods --install --namespace=${HELM_NAMESPACE_NAME} --set kubernetesNamespace=${HELM_NAMESPACE_NAME} --set releaseDateTime="$(date -u '+%Y-%m-%d %H-%M-%S')"  --set dockerImageTag=${DOCKER_IMAGE_TAG} --set replicaCount=${DESIRED_COUNT} ${SERVICE_NAME} -f ./chart-service/${SERVICE_NAME}/values.yaml ./chart-service/_common

```