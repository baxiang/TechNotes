```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.17.245.179:6443 --token bceyd9.ry4mjy0a5yl84xlh \
    --discovery-token-ca-cert-hash sha256:cafaaa16c4a38196409a87610edb4d9d0ec746946019fd6874f4245495f78e10
```
####下载dashboard文件：
```
curl -o kubernetes-dashboard.yaml  https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
````
修改文件
```
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  # 添加Service的type为NodePort
  type: NodePort
  ports:
    - port: 443
      targetPort: 8443
      # 添加映射到虚拟机的端口,k8s只支持30000以上的端口
      nodePort: 30001
  selector:
    k8s-app: kubernetes-dashboard
```
创建kubernetes-dashboard：
```
kubectl create -f kubernetes-dashboard.yaml
```
如果之前安装过
```
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": secrets "kubernetes-dashboard-certs" already exists
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": serviceaccounts "kubernetes-dashboard" already exists
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": roles.rbac.authorization.k8s.io "kubernetes-dashboard-minimal" already exists
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": rolebindings.rbac.authorization.k8s.io "kubernetes-dashboard-minimal" already exists
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": deployments.apps "kubernetes-dashboard" already exists
Error from server (AlreadyExists): error when creating "kubernetes-dashboard.yaml": services 
"kubernetes-dashboard" already exists
```
5. 卸载之前安装的内容：
```
kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```
6. 重新安装dashboard：
```
kubectl create -f kubernetes-dashboard.yaml
```

```
kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token

```
获取token
```


kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token


```

![image.png](https://upload-images.jianshu.io/upload_images/143845-54394da17abb96db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
卸载之前安装的内容



