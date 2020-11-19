kubectl get pods -o wide
kubectl get deployments
kubectl run nginx --image=nginx
kubectl delete pods <podname>
kubectl delete deployment <deploymentname>
kubectl delete service <servicename>

###update deployment
kubectl apply -f <deploymentfile.yml>

### rollout
kubectl set image deployment nginx nginx=nginx:1.8.1-alpine
kubectl describe pods nginx | grep -i image
kubectl rollout history deployment nginx --revision=1
kubectl rollout undo deployment nginx --to-revision=1 

### scaling
kubectl scale deployment <deploymentname> replicas=3
kubectl autoscale deployment nginx --min=2 --max=5 --cpu-percent=70

### port forward
kubectl port-forward <podname> 8000:80 &

### create service
kubectl expose deployment <deploymentname> --port=80 --type=NodePort
