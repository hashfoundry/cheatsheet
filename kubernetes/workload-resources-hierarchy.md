# Иерархия Workload Resources в Kubernetes

```
🚀 Workload Resources Ecosystem
│
├── 📦 Pod (Базовая единица выполнения)
│   ├── 📋 Containers
│   │   ├── 🐳 Application Container
│   │   ├── 🔧 Init Containers
│   │   └── 🛟 Sidecar Containers
│   ├── 💾 Volumes
│   │   ├── 📂 emptyDir
│   │   ├── 🔐 secret
│   │   ├── 📝 configMap
│   │   └── 💿 persistentVolumeClaim
│   ├── 🌐 Networking
│   │   ├── 🏷️ Pod IP
│   │   ├── 🔌 Ports
│   │   └── 🌍 DNS Name
│   ├── ⚙️ Configuration
│   │   ├── 📊 Resource Requests/Limits
│   │   ├── 🔒 Security Context
│   │   ├── 🏷️ Labels & Annotations
│   │   └── 🎯 Node Selector
│   └── 🔄 Lifecycle Hooks
│       ├── 🚀 PostStart
│       └── 🛑 PreStop
│
├── 🔄 ReplicaSet (Управление копиями Pod)
│   ├── 📋 Pod Template
│   ├── 🎯 Selector (labels)
│   ├── 🔢 Replicas Count
│   └── 📦 Managed Pods
│       ├── Pod-1
│       ├── Pod-2
│       └── Pod-N
│
├── 🚀 Deployment (Декларативные обновления)
│   ├── 🔄 Managed ReplicaSet
│   │   ├── 📋 Current ReplicaSet
│   │   └── 📋 Old ReplicaSets (during rollout)
│   ├── 📊 Rollout Strategy
│   │   ├── 🔄 Rolling Update
│   │   │   ├── maxSurge (25%)
│   │   │   └── maxUnavailable (25%)
│   │   └── 🔄 Recreate
│   ├── 📈 Scaling
│   │   ├── Manual Scaling
│   │   └── 🤖 HPA Integration
│   ├── 📜 Revision History
│   │   ├── RevisionHistoryLimit
│   │   └── 🔙 Rollback Capability
│   └── 🎯 Pod Template Spec
│
├── ⚡ StatefulSet (Stateful приложения)
│   ├── 📦 Ordered Pods
│   │   ├── app-0 (stable identity)
│   │   ├── app-1 (stable identity)
│   │   └── app-N (stable identity)
│   ├── 💾 Persistent Storage
│   │   ├── 📋 VolumeClaimTemplates
│   │   └── 💿 Persistent Volumes per Pod
│   ├── 🌐 Stable Network Identity
│   │   ├── 🏷️ Predictable Hostnames
│   │   └── 🌍 Stable DNS Names
│   ├── 📊 Update Strategy
│   │   ├── 🔄 Rolling Update
│   │   │   └── partition parameter
│   │   └── 🛑 OnDelete
│   └── 🎯 Service Requirement
│       └── Headless Service
│
├── 👥 DaemonSet (Узловые сервисы)
│   ├── 📦 Pod per Node
│   │   ├── 🖥️ Node-1 → Pod
│   │   ├── 🖥️ Node-2 → Pod
│   │   └── 🖥️ Node-N → Pod
│   ├── 🎯 Node Selection
│   │   ├── 🏷️ Node Selector
│   │   ├── 🧲 Node Affinity
│   │   └── 🏷️ Taints & Tolerations
│   ├── 📊 Update Strategy
│   │   ├── 🔄 Rolling Update
│   │   │   ├── maxUnavailable
│   │   └── 🛑 OnDelete
│   └── 💡 Use Cases
│       ├── 📊 Monitoring Agents
│       ├── 📝 Log Collectors
│       ├── 🌐 Network Plugins
│       └── 💾 Storage Drivers
│
├── ⚙️ Job (Одноразовые задачи)
│   ├── 📦 Task Pods
│   │   ├── Pod-1 (task)
│   │   ├── Pod-2 (task) [if parallelism > 1]
│   │   └── Pod-N (task)
│   ├── 🎯 Completion Criteria
│   │   ├── 🔢 Completions (desired)
│   │   ├── 🔄 Parallelism (concurrent)
│   │   └── ⏱️ Active Deadline Seconds
│   ├── 🔄 Restart Policy
│   │   ├── Never
│   │   └── OnFailure
│   ├── 🛡️ Failure Handling
│   │   ├── 🔢 Backoff Limit
│   │   └── 🔄 Pod Failure Policy
│   └── 🧹 Cleanup
│       └── TTL After Finished
│
├── ⏰ CronJob (Запланированные задачи)
│   ├── 📅 Schedule (Cron format)
│   │   ├── "0 2 * * *" (daily at 2 AM)
│   │   ├── "*/5 * * * *" (every 5 minutes)
│   │   └── "@weekly" (shortcuts)
│   ├── ⚙️ Job Template
│   │   └── → Creates Job instances
│   ├── 🕐 Execution Policies
│   │   ├── 🚫 Concurrency Policy
│   │   │   ├── Allow (default)
│   │   │   ├── Forbid
│   │   │   └── Replace
│   │   ├── ⏰ Starting Deadline Seconds
│   │   └── 🧹 Successful/Failed Jobs History Limit
│   └── 🔄 Job History
│       ├── Successful Jobs (keep 3)
│       └── Failed Jobs (keep 1)
│
└── 🎮 Controllers & Operators
    ├── 🤖 Built-in Controllers
    │   ├── Deployment Controller
    │   ├── ReplicaSet Controller
    │   ├── StatefulSet Controller
    │   ├── DaemonSet Controller
    │   └── Job/CronJob Controller
    ├── 📋 Custom Resource Definitions (CRDs)
    └── 🔧 Custom Controllers/Operators
        ├── Application-specific logic
        └── Extended workload patterns
```

## Связи и отношения между Workload Resources:

### 🔗 Иерархические связи:
```
CronJob ──creates──> Job ──creates──> Pod
Deployment ──creates──> ReplicaSet ──creates──> Pod
StatefulSet ──creates──> Pod (ordered)
DaemonSet ──creates──> Pod (one per node)
```

### 🎯 Селекторы и Labels:
```
Workload Resource
├── spec.selector ──matches──> Pod labels
├── spec.template.metadata.labels ──applied to──> Created Pods
└── metadata.labels ──used by──> Services & Ingress
```

### 🌐 Сервисные связи:
```
Service ──selects──> Pods (via labels)
├── ClusterIP ──load balances──> Pod IPs
├── Headless Service ──direct access──> StatefulSet Pods
└── Ingress ──routes to──> Services ──routes to──> Pods
```

### 💾 Хранилище связи:
```
StatefulSet ──uses──> VolumeClaimTemplate ──creates──> PVC ──binds to──> PV
Pod ──mounts──> Volumes
├── PVC Volumes ──persistent data
├── ConfigMap Volumes ──configuration
├── Secret Volumes ──sensitive data
└── EmptyDir Volumes ──temporary data
```

### 📊 Масштабирование связи:
```
HPA ──scales──> Deployment/ReplicaSet/StatefulSet
VPA ──adjusts resources──> Pod containers
Cluster Autoscaler ──scales──> Nodes ──affects──> Pod scheduling
```

### 🎮 Управление связи:
```
kubectl ──communicates with──> API Server
API Server ──stores state in──> etcd
Controllers ──watch──> API Server ──create/update/delete──> Resources
Scheduler ──assigns──> Pods to Nodes
kubelet ──runs──> Pods on Nodes
```

## 🔄 Жизненный цикл Workload Resources:

### 1️⃣ Создание:
```
User/CI/CD ──applies YAML──> API Server ──validates──> etcd
Controller ──watches changes──> creates child resources
```

### 2️⃣ Планирование:
```
Scheduler ──selects Node for──> unscheduled Pods
kubelet ──pulls images & starts──> Containers
```

### 3️⃣ Выполнение:
```
Pod ──runs on──> Node
Container Runtime ──manages──> Container lifecycle
kubelet ──reports status to──> API Server
```

### 4️⃣ Мониторинг:
```
Probes ──check──> Pod health
Controllers ──ensure desired state
Metrics ──collected by──> monitoring systems
```

### 5️⃣ Обновление:
```
User ──updates spec──> Controller ──performs rolling update
Old Pods ──gracefully terminated
New Pods ──created with new spec
```

### 6️⃣ Очистка:
```
User ──deletes resource
Controller ──cleans up child resources
Pods ──terminated gracefully
Volumes ──cleaned up based on policy
```

## 📋 Сравнение Workload Resources:

| Resource | Use Case | Pod Management | Scaling | Updates | Storage |
|----------|----------|----------------|---------|---------|---------|
| **Pod** | Single instance | Direct | Manual | Replace | Ephemeral |
| **ReplicaSet** | Identical replicas | Template-based | Manual/HPA | Replace | Ephemeral |
| **Deployment** | Stateless apps | Via ReplicaSet | Manual/HPA | Rolling/Recreate | Ephemeral |
| **StatefulSet** | Stateful apps | Ordered identity | Manual/HPA | Rolling/OnDelete | Persistent |
| **DaemonSet** | Node services | One per node | Node-based | Rolling/OnDelete | Mixed |
| **Job** | Batch tasks | Task completion | Parallelism | N/A | Task-specific |
| **CronJob** | Scheduled tasks | Via Jobs | Time-based | N/A | Task-specific |
