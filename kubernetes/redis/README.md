# redis
学习笔记

* 使用文件创建，文件名： learn-redis.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: my-learn
  name: learn-redis-deployment
  labels:
    app: learn-redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: learn-redis
  template:
    metadata:
      labels:
        app: learn-redis
    spec:
      containers:
      - name: redis
        image: redis:5.0.6
        ports:
        - containerPort: 6379

---
apiVersion: v1
kind: Service
metadata:
  namespace: my-learn
  name: learn-redis-service
spec:
  selector:
    app: learn-redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
      nodePort: 32379
  type: NodePort

```
