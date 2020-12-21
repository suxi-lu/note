# namespace

学习笔记

### 官方文档地址： [https://kubernetes.io/docs/tasks/administer-cluster/namespaces/](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/)

## 1. 查看 namespace

* 查看所有： `kubectl get namespaces`
* 查看namespace信息：`kubectl get namespaces my-learn` or `kubectl describe namespaces my-learn`
* 列出所有：`kubectl get namespaces --show-labels`

## 2. 创建 namespace

* 使用文件创建，文件名： namespace-my-learn.yaml

  ```text
  apiVersion: v1
  kind: Namespace
  metadata:
  name: my-learn
  ```

`# kubectl create -f ./my-namespace.yaml`

* 使用命令创建 `kubectl create namespace my-learn`

## 3. 删除 namespace

`kubectl delete namespaces my-learn`

## 4. 查看当前上下命名空间上下文信息

* `kubectl config view`
* `kubectl config current-context`
* 查看所有上下文空间： `kubectl config get-contexts`

## 5. 创建上下文空间

* `kubectl config set-context my-learn --namespace=my-learn --cluster=kubernetes --user=kubernetes-admin`
* 检查命名空间 `kubectl config get-contexts`

## 6. 切换上下文空间

* `kubectl config use-context my-learn`
* 检查命名空间 `kubectl config get-contexts`

