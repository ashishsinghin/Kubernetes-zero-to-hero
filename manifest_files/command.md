# 04-ConfigMaps and Secrets

## Configmap
`kubectl apply -f manifest_files/04-configmap.yaml`
`kubectl get configmap`
`kubectl apply -f manifest_files/04-env-configmap.yaml`
`kubectl logs pod/env-configmap `

`kubectl create configmap my-config --from-literal=username=k8s-admin --from-literal=access_level=1`
`kubectl describe configmap my-config`
`kubectl get configmap my-config -o yaml`
`kubectl apply -f manifest_files/04-env-configmap-2.yaml`
`kubectl logs pod/ env-configmap`
`kubectl describe pod env-configmap`

## Secret

`kubectl apply -f manifest_files/04-secret.yaml`
`kubectl get secret my-secret -o yaml`
`echo "dXNlcm5hbWU=" | base64 --decode`
`echo "cGFzc3dvcmQ=" | base64 --decode`
`kubectl get secret my-secret -o json | jq '.data | map_values(@base64d)'`
`kubectl apply -f manifest_files/04-pod-secret-2.yaml`
`kubectl exec -it secret-test-pod -- /bin/sh -c 'echo $SECRET_USERNAME'`
`kubectl exec -it secret-test-pod -- /bin/sh -c 'echo $SECRET_PASSWORD'`
`kubectl delete -f manifest_files/04-pod-secret-2.yaml`
`kubectl delete -f manifest_files/04-secret.yaml`


`kubectl create secret generic backend-user --from-literal=backend-username='backend-admin'`
`kubectl get secret`
`kubectl get secret backend-user -o yaml`
`kubectl apply -f manifest_files/04-pod-secret.yaml`
`kubectl exec -i -t env-single-secret -- /bin/sh -c 'echo $SECRET_USERNAME'`
`kubectl delete -f manifest_files/04-pod-secret.yaml`
`kubectl delete secret backend-user`


===========================================

Use kubectl to apply the manifest file:
`kubectl apply -f manifest_files/04-demo-configmap-secret.yaml`

Verify the Resources
Check if the ConfigMap, Secret, and Pod are created successfully:

```bash
kubectl get configmap demo-config
kubectl get secret demo-secret
kubectl get pod demo-pod
```
Inspect the Pod
To view the Pod's logs or verify environment variables and mounted files, use the following commands:

Check Environment Variables:

```bash
kubectl exec demo-pod -- env
```

Inspect Mounted ConfigMap Files:

```bash
kubectl exec demo-pod -- cat /etc/config/app.properties
kubectl exec demo-pod -- cat /etc/config/environment
```

Inspect Mounted Secret Files:

```bash
kubectl exec demo-pod -- ls /etc/secret
kubectl exec demo-pod -- cat /etc/secret/username
kubectl exec demo-pod -- cat /etc/secret/password
```
