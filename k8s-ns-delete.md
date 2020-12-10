### 1. This command (with kubectl 1.11+) will show you what resources remain in the namespace:

```sh
  $ kubectl api-resources --verbs=list --namespaced -o name \
  | xargs -n 1 kubectl get --show-kind --ignore-not-found -n <namespace>
```  
  
### 2. Force-delete the namespace (perhaps leaving dangling resources):

```sh
  $ NAMESPACE=your-rogue-namespace
  
  $ kubectl proxy &  (for auth purposes)
  
  $ kubectl get namespace $NAMESPACE -o json |jq '.spec = {"finalizers":[]}' > temp.json
  
  $ curl -k -H "Content-Type: application/json" -X PUT --data-binary @temp.json 127.0.0.1:8001/api/v1/namespaces/$NAMESPACE/finalize
  
```
  
 ### 3. This way you will not have to spawn a kubectl proxy process and avoid dependency with curl 
 
 ```sh
  kubectl get namespace "stucked-namespace" -o json \
  | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
  | kubectl replace --raw /api/v1/namespaces/stucked-namespace/finalize -f -

 
 ```
