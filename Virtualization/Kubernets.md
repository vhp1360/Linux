<div dir=rtl>بنام خدا</div>

###### top

- [MiniKube](#minikube)
- [kubectl](#kubctl)


# Minikube
- Some Commands
```vala
  minikube start
  
```



[Top](#top)
# Kubectl
- Some Commands
```vala
  kubectl start
  kubctl cluster-info
  kubctl get nodes
  kubctl get nodes --help
  kubectl run ImageName --image=/Path/To/Image:version --port=8080
  kubectl get deployments
  kubectl proxy
  export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
  echo Name of the Pod: $POD_NAME
  curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME/
  

```





[Top](#top)
#



[Top](#top)
#
