# Иерархия сущностей Kubernetes

```
📦 Kubernetes Cluster
├── 🎯 Control Plane (Master Node)
│   ├── 🌐 API Server (kube-apiserver)
│   ├── 🗄️ etcd (хранилище состояния)
│   ├── 📋 Scheduler (kube-scheduler)
│   ├── 🎮 Controller Manager (kube-controller-manager)
│   ├── ☁️ Cloud Controller Manager
│   └── 🔧 Addons
│       ├── 🌍 CoreDNS
│       ├── 📊 Dashboard
│       └── 🚪 Ingress Controller
│
├── 💻 Worker Nodes
│   ├── 🤖 kubelet
│   ├── 🔄 kube-proxy
│   ├── 🐳 Container Runtime (Docker/containerd/CRI-O)
│   └── 📦 Pods
│       └── 📋 Containers
│
├── 🏷️ Namespaces
│   ├── default
│   ├── kube-system
│   ├── kube-public
│   └── custom namespaces
│
├── 🚀 Workload Resources
│   ├── 📦 Pod (базовая единица)
│   ├── 🔄 ReplicaSet
│   ├── 🚀 Deployment
│   ├── ⚡ StatefulSet
│   ├── 👥 DaemonSet
│   ├── ⚙️ Job
│   └── ⏰ CronJob
│
├── 🌐 Network Resources
│   ├── 🔗 Service
│   │   ├── ClusterIP
│   │   ├── NodePort
│   │   ├── LoadBalancer
│   │   └── ExternalName
│   ├── 🚪 Ingress
│   ├── 🔚 Endpoints
│   └── 🔒 Network Policy
│
├── 💾 Storage Resources
│   ├── 📀 Persistent Volume (PV)
│   ├── 📋 Persistent Volume Claim (PVC)
│   ├── 🏪 Storage Class
│   └── 📂 Volume
│       ├── emptyDir
│       ├── hostPath
│       ├── configMap
│       ├── secret
│       └── persistentVolumeClaim
│
├── ⚙️ Configuration Resources
│   ├── 📝 ConfigMap
│   ├── 🔐 Secret
│   ├── 🏷️ Labels
│   ├── 🎯 Selectors
│   └── 📋 Annotations
│
├── 🔐 Security & Access Control
│   ├── 👤 Service Account
│   ├── 🎭 Role
│   ├── 🌍 ClusterRole
│   ├── 🔗 RoleBinding
│   ├── 🌐 ClusterRoleBinding
│   ├── 🛡️ Pod Security Policy
│   └── 🚧 Network Policy
│
├── 📊 Scaling & Scheduling
│   ├── 📈 Horizontal Pod Autoscaler (HPA)
│   ├── 📊 Vertical Pod Autoscaler (VPA)
│   ├── 🔧 Cluster Autoscaler
│   ├── 🧲 Node Affinity
│   ├── 🤝 Pod Affinity/Anti-Affinity
│   ├── 🏷️ Taints & Tolerations
│   └── 📊 Priority Classes
│
└── 🔧 Custom Resources
    ├── 📋 Custom Resource Definition (CRD)
    ├── 🤖 Operators
    ├── 🔧 Custom Controllers
    └── 📦 Helm Charts
```

## Основные отношения:

1. **Cluster → Nodes**: Кластер состоит из узлов (master и worker)
2. **Nodes → Pods**: Узлы запускают поды
3. **Pods → Containers**: Поды содержат контейнеры
4. **Workloads → Pods**: Ресурсы рабочей нагрузки управляют подами
5. **Services → Pods**: Сервисы предоставляют доступ к подам
6. **Namespaces → Resources**: Пространства имен группируют ресурсы
7. **ConfigMaps/Secrets → Pods**: Конфигурация инжектируется в поды
8. **PVC → PV**: Заявки на хранилище связываются с томами
9. **RBAC → Resources**: Контроль доступа применяется к ресурсам

## Иерархия по уровням:

### Уровень 1: Инфраструктура
- Cluster
- Nodes (Master/Worker)

### Уровень 2: Среда выполнения
- Container Runtime
- kubelet, kube-proxy

### Уровень 3: Логическая организация
- Namespaces
- Labels, Selectors

### Уровень 4: Рабочая нагрузка
- Pods
- Deployments, StatefulSets, DaemonSets

### Уровень 5: Сетевые сервисы
- Services
- Ingress
- Network Policies

### Уровень 6: Хранилище
- Persistent Volumes
- Storage Classes

### Уровень 7: Конфигурация и безопасность
- ConfigMaps, Secrets
- RBAC, Service Accounts

### Уровень 8: Автомасштабирование и планирование
- HPA, VPA, Cluster Autoscaler
- Node/Pod Affinity, Taints/Tolerations
