## K8s
Restart all pods in a namespace
```bash
kubectl -n <namespace> rollout restart deploy
```

Enter Shell in a pod
```bash
kubectl exec --stdin --tty <pod-name> -- /bin/bash
```

Force Delete a pod
```bash
kubectl delete pods <pod> --grace-period=0 --force
```

Get Events ordered by timestamp
```bash
 kubectl get events --all-namespaces  --sort-by='.metadata.creationTimestamp'
```


## K8s Flag
You can list Kubernetes node details along with their labels in this fashion:
```bash
kubectl get nodes --show-labels
```

If you want to know the details for a specific node, use this:
```bash
kubectl label --list nodes node_name
```

Let's label that node with an appropriate name (like production):
```bash
kubectl label nodes <node-name> <label-name>=<label-value>
```

If you later decide to overwrite some labels based on the requirements see how you can achieve that.
```bash
kubectl label --overwrite nodes <node-name> <label-name>=<label-value>
```

To remove the label from a node, provide the key without any value.
```bash
kubectl label --overwrite nodes <node-name> <label-name>-
```
