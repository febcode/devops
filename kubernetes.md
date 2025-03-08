You can use the following command to execute `curl` from within a pod using `kubectl exec`:  

```sh
kubectl exec -it <pod-name> -- curl -v <URL>
```

### **Example Usage**
```sh
kubectl exec -it my-pod -- curl -v http://example.com
```

### **If the Pod is Running in a Specific Namespace**
```sh
kubectl exec -it my-pod -n my-namespace -- curl -v http://example.com
```

### **If the Pod Has Multiple Containers**
```sh
kubectl exec -it my-pod -c my-container -- curl -v http://example.com
```

### **If You Need to Install `curl` Inside the Pod**
If `curl` is not available inside the pod, you can try using `wget`:
```sh
kubectl exec -it my-pod -- wget -O- http://example.com
```

Or, if the pod allows, install `curl` (for Debian-based containers):
```sh
kubectl exec -it my-pod -- apt update && apt install curl -y
```

