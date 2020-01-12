# mysql
学习笔记

* 使用文件创建，文件名： learn-mysql.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: my-learn
  name: learn-mysql-deployment
  labels:
    app: learn-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: learn-mysql
  template:
    metadata:
      labels:
        app: learn-mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0.18
        ports:
        - containerPort: 3306
        args: ["--default-authentication-plugin=mysql_native_password", "--character-set-server=utf8mb4", "--collation-server=utf8mb4_unicode_ci"]
        env: 
        - name: "MYSQL_ROOT_PASSWORD"
          value: "123456"
        - name: "MYSQL_USER"
          value: "learn"
        - name: "MYSQL_PASSWORD"
          value: "learn-m"

---
apiVersion: v1
kind: Service
metadata:
  namespace: my-learn
  name: learn-mysql-service
spec:
  selector:
    app: learn-mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
      nodePort: 32306
  type: NodePort

```
