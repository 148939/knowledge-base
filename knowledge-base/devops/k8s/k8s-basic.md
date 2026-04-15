# Kubernetes 基础入门

## 1. 核心概述（官方定义）

Kubernetes（K8s）是一个开源的容器编排平台，用于自动化部署、扩展和管理容器化应用程序。它由 Google 最初设计和开发，后来捐赠给云原生计算基金会（CNCF）。

### 核心概念
- **Pod**：K8s 中最小的可部署单元
- **Node**：集群中的工作节点
- **Cluster**：由一组节点组成的集群
- **Namespace**：命名空间，用于资源隔离
- **Service**：服务，定义访问 Pod 的策略
- **Deployment**：部署，声明式更新 Pod 和 ReplicaSet
- **ConfigMap/Secret**：配置管理
- **Volume**：存储卷

---

## 2. 核心知识点（结构化层级）

### 2.1 Kubernetes 架构

#### Master 组件
- **kube-apiserver**：API 服务器，集群入口
- **etcd**：分布式键值存储，集群数据库
- **kube-scheduler**：调度器，分配 Pod 到节点
- **kube-controller-manager**：控制器管理器，管理各种控制器
- **cloud-controller-manager**：云控制器管理器，与云提供商集成

#### Node 组件
- **kubelet**：节点代理，管理 Pod 生命周期
- **kube-proxy**：网络代理，维护网络规则
- **Container Runtime**：容器运行时（Docker、containerd）

#### 插件
- **DNS**：集群 DNS 服务
- **Ingress Controller**：入口控制器
- **Metric Server**：指标服务器
- **Dashboard**：Web UI

### 2.2 核心资源对象

#### Pod
- Pod 定义
- Pod 生命周期
- 重启策略
- 健康检查（Liveness/Readiness Probe）
- Init Container
- 资源限制（Requests/Limits）

#### Deployment
- Deployment 定义
- 更新策略（Recreate/RollingUpdate）
- 回滚操作
- 扩缩容
- 暂停/恢复

#### Service
- Service 类型（ClusterIP/NodePort/LoadBalancer/ExternalName）
- 服务发现
- 端口转发
- Endpoint

#### Ingress
- Ingress 定义
- Ingress Controller
- 路由规则
- TLS/SSL 配置

### 2.3 配置管理

#### ConfigMap
- 创建 ConfigMap
- 使用 ConfigMap
- 环境变量注入
- 配置文件挂载

#### Secret
- 创建 Secret
- 使用 Secret
- 加密敏感数据
- 镜像拉取凭证

### 2.4 存储

#### Volume
- 临时卷（emptyDir）
- 持久卷（PersistentVolume）
- 持久卷声明（PersistentVolumeClaim）
- StorageClass

#### 存储类型
- HostPath
- NFS
- CSI（容器存储接口）
- 云存储（AWS EBS、GCE PD）

### 2.5 网络

#### 网络模型
- Pod 网络
- Service 网络
- Cluster DNS
- 网络策略（NetworkPolicy）

#### CNI 插件
- Flannel
- Calico
- Canal
- Cilium

### 2.6 调度与亲和性

#### 调度策略
- 节点选择器（nodeSelector）
- 节点亲和性（nodeAffinity）
- Pod 亲和性（podAffinity）
- Pod 反亲和性（podAntiAffinity）

#### 污点与容忍
- 污点（Taint）
- 容忍（Toleration）
- 节点隔离

### 2.7 安全

#### RBAC
- Role
- ClusterRole
- RoleBinding
- ClusterRoleBinding

#### 安全上下文
- Pod 安全上下文
- 容器安全上下文
- SecurityContextConstraint

---

## 3. 官方标准语法 / API

### 3.1 Pod 定义

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: default
  labels:
    app: my-app
    env: production
spec:
  containers:
  - name: my-container
    image: nginx:1.21
    imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 80
      name: http
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    livenessProbe:
      httpGet:
        path: /health
        port: 80
      initialDelaySeconds: 30
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /ready
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  volumes:
  - name: config-volume
    configMap:
      name: my-config
  restartPolicy: Always
```

### 3.2 Deployment 定义

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  namespace: default
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: nginx:1.21
        ports:
        - containerPort: 80
```

### 3.3 Service 定义

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: default
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    name: http

---
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080

---
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

### 3.4 Ingress 定义

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
  tls:
  - hosts:
    - my-app.example.com
    secretName: my-tls-secret
```

### 3.5 ConfigMap 定义

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
  namespace: default
data:
  database_host: mysql-service
  database_port: "3306"
  log_level: info
  app-config.json: |
    {
      "database": {
        "host": "mysql-service",
        "port": 3306
      },
      "logging": {
        "level": "info"
      }
    }

---
# 在 Pod 中使用 ConfigMap
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:1.21
    env:
    - name: DATABASE_HOST
      valueFrom:
        configMapKeyRef:
          name: my-config
          key: database_host
    - name: LOG_LEVEL
      valueFrom:
        configMapKeyRef:
          name: my-config
          key: log_level
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: my-config
```

### 3.6 Secret 定义

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
  namespace: default
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=

---
# 在 Pod 中使用 Secret
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:1.21
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: password
  imagePullSecrets:
  - name: regcred
```

### 3.7 PersistentVolume 和 PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/data

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual

---
# 在 Pod 中使用 PVC
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:1.21
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: my-storage
  volumes:
  - name: my-storage
    persistentVolumeClaim:
      claimName: my-pvc
```

### 3.8 常用命令

```bash
# 集群信息
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl describe node <node-name>

# Namespace 操作
kubectl get namespaces
kubectl create namespace my-namespace
kubectl config set-context --current --namespace=my-namespace

# Pod 操作
kubectl get pods
kubectl get pods -o wide
kubectl get pods -n <namespace>
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs -f <pod-name>
kubectl logs <pod-name> -c <container-name>
kubectl exec -it <pod-name> -- /bin/bash
kubectl port-forward <pod-name> 8080:80

# Deployment 操作
kubectl get deployments
kubectl describe deployment <deployment-name>
kubectl scale deployment <deployment-name> --replicas=5
kubectl set image deployment/<deployment-name> <container>=<image>
kubectl rollout status deployment/<deployment-name>
kubectl rollout history deployment/<deployment-name>
kubectl rollout undo deployment/<deployment-name>
kubectl rollout undo deployment/<deployment-name> --to-revision=2

# Service 操作
kubectl get services
kubectl describe service <service-name>
kubectl expose deployment <deployment-name> --type=NodePort --port=80

# 资源创建和删除
kubectl apply -f <file.yaml>
kubectl delete -f <file.yaml>
kubectl delete pod <pod-name>
kubectl delete deployment <deployment-name>
kubectl delete service <service-name>

# 配置管理
kubectl get configmaps
kubectl create configmap my-config --from-literal=key1=value1 --from-file=config.json
kubectl get secrets
kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=secret

# 标签和注解
kubectl label pods <pod-name> app=my-app env=prod
kubectl annotate pods <pod-name> description="My pod"
```

---

## 4. 官方示例代码（可运行）

### 4.1 完整的 Web 应用部署

```yaml
# Namespace
apiVersion: v1
kind: Namespace
metadata:
  name: web-app

---
# ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: web-app-config
  namespace: web-app
data:
  DB_HOST: "mysql-service"
  DB_PORT: "3306"
  REDIS_HOST: "redis-service"
  REDIS_PORT: "6379"

---
# Secret
apiVersion: v1
kind: Secret
metadata:
  name: web-app-secret
  namespace: web-app
type: Opaque
data:
  DB_USERNAME: YWRtaW4=
  DB_PASSWORD: cGFzc3dvcmQ=
  REDIS_PASSWORD: cmVkaXM=

---
# MySQL Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  namespace: web-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: web-app-secret
              key: DB_PASSWORD
        - name: MYSQL_DATABASE
          value: "app_db"
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pvc

---
# MySQL PVC
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
  namespace: web-app
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi

---
# MySQL Service
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
  namespace: web-app
spec:
  selector:
    app: mysql
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306

---
# Redis Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: web-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:7-alpine
        command: ["redis-server", "--requirepass", "$(REDIS_PASSWORD)"]
        env:
        - name: REDIS_PASSWORD
          valueFrom:
            secretKeyRef:
              name: web-app-secret
              key: REDIS_PASSWORD
        ports:
        - containerPort: 6379

---
# Redis Service
apiVersion: v1
kind: Service
metadata:
  name: redis-service
  namespace: web-app
spec:
  selector:
    app: redis
  ports:
  - protocol: TCP
    port: 6379
    targetPort: 6379

---
# Web App Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  namespace: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-app
        image: my-web-app:1.0.0
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: web-app-config
              key: DB_HOST
        - name: DB_PORT
          valueFrom:
            configMapKeyRef:
              name: web-app-config
              key: DB_PORT
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: web-app-secret
              key: DB_USERNAME
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: web-app-secret
              key: DB_PASSWORD
        - name: REDIS_HOST
          valueFrom:
            configMapKeyRef:
              name: web-app-config
              key: REDIS_HOST
        - name: REDIS_PORT
          valueFrom:
            configMapKeyRef:
              name: web-app-config
              key: REDIS_PORT
        - name: REDIS_PASSWORD
          valueFrom:
            secretKeyRef:
              name: web-app-secret
              key: REDIS_PASSWORD
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5

---
# Web App Service
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
  namespace: web-app
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080

---
# Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app-ingress
  namespace: web-app
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: web-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-app-service
            port:
              number: 80
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 资源管理

1. **设置资源限制**
   - 始终设置 requests 和 limits
   - 合理分配 CPU 和内存
   - 避免资源争抢

2. **使用 Namespace 隔离**
   - 按环境隔离（dev/staging/prod）
   - 按团队或应用隔离
   - 应用 ResourceQuota

3. **标签和注解**
   - 合理使用标签进行分类
   - 使用注解记录元数据
   - 便于查询和管理

### 5.2 部署策略

1. **使用 Deployment**
   - 推荐使用 Deployment 管理 Pod
   - 利用滚动更新策略
   - 支持回滚操作

2. **健康检查**
   - 配置 livenessProbe
   - 配置 readinessProbe
   - 合理设置探测参数

3. **ConfigMap 和 Secret**
   - 将配置与代码分离
   - 使用 Secret 存储敏感数据
   - 考虑使用 External Secrets Operator

### 5.3 安全实践

1. **RBAC**
   - 最小权限原则
   - 使用 Role 和 RoleBinding
   - 定期审计权限

2. **镜像安全**
   - 使用官方或可信镜像
   - 定期扫描镜像漏洞
   - 避免使用 latest 标签

3. **网络策略**
   - 配置 NetworkPolicy
   - 限制 Pod 间通信
   - 遵循最小暴露原则

### 5.4 运维最佳实践

1. **监控和日志**
   - 部署 Prometheus + Grafana
   - 收集和分析日志（ELK/EFK）
   - 设置告警规则

2. **备份和恢复**
   - 定期备份 etcd
   - 备份重要配置
   - 测试恢复流程

3. **升级策略**
   - 逐步升级集群
   - 先在测试环境验证
   - 准备回滚方案

---

## 6. 官方文档参考链接

- [Kubernetes 官方文档](https://kubernetes.io/docs/)
- [Kubernetes 入门指南](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Kubernetes API 参考](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.28/)
- [kubectl 命令参考](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
- [Kubernetes 最佳实践](https://kubernetes.io/docs/concepts/configuration/overview/)
- [Helm 文档](https://helm.sh/docs/)
- [Kustomize 文档](https://kustomize.io/)
