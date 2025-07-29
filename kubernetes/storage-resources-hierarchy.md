# Иерархия Storage Resources в Kubernetes

```
💾 Kubernetes Storage Ecosystem
│
├── 🏪 Storage Classes (Dynamic Provisioning)
│   ├── 📋 Provisioner
│   │   ├── kubernetes.io/aws-ebs
│   │   ├── kubernetes.io/gce-pd
│   │   ├── kubernetes.io/azure-disk
│   │   ├── csi.driver.name (CSI)
│   │   └── Custom Provisioners
│   ├── ⚙️ Parameters
│   │   ├── type: gp2, io1, sc1
│   │   ├── replication-type: LRS, ZRS
│   │   ├── encrypted: true/false
│   │   └── Custom Parameters
│   ├── 🔄 Reclaim Policy
│   │   ├── Delete (default)
│   │   ├── Retain
│   │   └── Recycle (deprecated)
│   ├── 📊 Volume Binding Mode
│   │   ├── Immediate
│   │   └── WaitForFirstConsumer
│   ├── 🔧 Allow Volume Expansion
│   └── 📍 Allowed Topologies
│
├── 💿 Persistent Volumes (PV) - Cluster Resources
│   ├── 🏷️ Capacity
│   │   └── storage: 10Gi, 100Gi, 1Ti
│   ├── 🔐 Access Modes
│   │   ├── ReadWriteOnce (RWO) - single node
│   │   ├── ReadOnlyMany (ROX) - multiple nodes read
│   │   └── ReadWriteMany (RWX) - multiple nodes read/write
│   ├── 🔄 Reclaim Policy
│   │   ├── Retain - manual cleanup
│   │   ├── Delete - auto cleanup
│   │   └── Recycle - deprecated
│   ├── 📊 Volume Mode
│   │   ├── Filesystem (default)
│   │   └── Block
│   ├── 🏪 Storage Class Reference
│   ├── 🔧 Volume Source
│   │   ├── 🌍 Cloud Volumes
│   │   │   ├── awsElasticBlockStore
│   │   │   ├── gcePersistentDisk
│   │   │   ├── azureDisk
│   │   │   └── cinder (OpenStack)
│   │   ├── 🖥️ Local Storage
│   │   │   ├── hostPath
│   │   │   ├── local
│   │   │   └── emptyDir
│   │   ├── 🌐 Network Storage
│   │   │   ├── nfs
│   │   │   ├── iscsi
│   │   │   ├── rbd (Ceph)
│   │   │   ├── cephfs
│   │   │   ├── glusterfs
│   │   │   └── portworxVolume
│   │   └── 🔌 CSI Volumes
│   │       └── csi.driver.name
│   ├── 📍 Node Affinity
│   └── 🏷️ Mount Options
│
├── 📋 Persistent Volume Claims (PVC) - Namespace Resources
│   ├── 🏷️ Resource Request
│   │   └── storage: 5Gi, 20Gi
│   ├── 🔐 Access Modes (requested)
│   │   ├── ReadWriteOnce
│   │   ├── ReadOnlyMany
│   │   └── ReadWriteMany
│   ├── 🏪 Storage Class Name
│   │   ├── Specified class
│   │   ├── "" (default class)
│   │   └── nil (no dynamic provisioning)
│   ├── 🎯 Selector (optional)
│   │   ├── matchLabels
│   │   └── matchExpressions
│   ├── 📊 Volume Mode
│   │   ├── Filesystem
│   │   └── Block
│   └── 📈 Volume Expansion
│       └── Resize capability
│
├── 📂 Volume Types (Pod Level)
│   ├── 🔧 Persistent Volumes
│   │   └── persistentVolumeClaim
│   │       ├── claimName: my-pvc
│   │       └── readOnly: true/false
│   ├── 📝 Configuration Volumes
│   │   ├── configMap
│   │   │   ├── name: config-name
│   │   │   ├── items: specific keys
│   │   │   ├── defaultMode: 0644
│   │   │   └── optional: true/false
│   │   └── secret
│   │       ├── secretName: secret-name
│   │       ├── items: specific keys
│   │       ├── defaultMode: 0600
│   │       └── optional: true/false
│   ├── 💾 Temporary Volumes
│   │   ├── emptyDir
│   │   │   ├── medium: "" (node storage)
│   │   │   ├── medium: "Memory" (tmpfs)
│   │   │   └── sizeLimit: 1Gi
│   │   └── downwardAPI
│   │       ├── fieldRef: metadata.labels
│   │       └── resourceFieldRef: limits.memory
│   ├── 🖥️ Host Volumes
│   │   ├── hostPath
│   │   │   ├── path: /var/log
│   │   │   └── type: Directory, File, Socket
│   │   └── local
│   │       ├── path: /mnt/ssd
│   │       └── nodeAffinity required
│   ├── 🌐 Network Volumes
│   │   ├── nfs
│   │   │   ├── server: nfs-server.example.com
│   │   │   ├── path: /exported/path
│   │   │   └── readOnly: true/false
│   │   ├── iscsi
│   │   │   ├── targetPortal: 10.0.2.15:3260
│   │   │   ├── iqn: iqn.2001-04.com.example:storage
│   │   │   └── lun: 0
│   │   └── cephfs
│   │       ├── monitors: ["10.16.154.78:6789"]
│   │       ├── path: /
│   │       └── secretRef: ceph-secret
│   └── 🔌 CSI Volumes
│       ├── csi
│       │   ├── driver: ebs.csi.aws.com
│       │   ├── volumeHandle: vol-12345
│       │   ├── fsType: ext4
│       │   └── volumeAttributes
│       └── ephemeral
│           ├── driver: example.com/csi
│           └── volumeAttributes
│
├── 🔌 Container Storage Interface (CSI)
│   ├── 🚗 CSI Drivers
│   │   ├── 🌍 Cloud Providers
│   │   │   ├── ebs.csi.aws.com (AWS EBS)
│   │   │   ├── pd.csi.storage.gke.io (GCP)
│   │   │   ├── disk.csi.azure.com (Azure)
│   │   │   └── cinder.csi.openstack.org
│   │   ├── 💾 Storage Systems
│   │   │   ├── rook-ceph.rbd.csi.ceph.com
│   │   │   ├── longhorn.rancher.io
│   │   │   ├── portworx.com/csi
│   │   │   └── pure-csi.purestorage.com
│   │   └── 🖥️ Local Storage
│   │       ├── local.csi.k8s.io
│   │       └── hostpath.csi.k8s.io
│   ├── 🧩 CSI Components
│   │   ├── 🎮 CSI Controller
│   │   │   ├── CreateVolume
│   │   │   ├── DeleteVolume
│   │   │   ├── ControllerPublishVolume
│   │   │   └── ListVolumes
│   │   ├── 🤖 CSI Node
│   │   │   ├── NodeStageVolume
│   │   │   ├── NodePublishVolume
│   │   │   └── NodeGetCapabilities
│   │   └── 🔍 CSI Identity
│   │       ├── GetPluginInfo
│   │       ├── GetPluginCapabilities
│   │       └── Probe
│   ├── 📋 CSI Objects
│   │   ├── CSIDriver
│   │   ├── CSINode
│   │   └── VolumeAttachment
│   └── 🔧 CSI Features
│       ├── Dynamic Provisioning
│       ├── Pre-provisioned Volumes
│       ├── Volume Expansion
│       ├── Volume Snapshots
│       └── Volume Cloning
│
├── 📸 Volume Snapshots
│   ├── 📋 VolumeSnapshotClass
│   │   ├── driver: ebs.csi.aws.com
│   │   ├── deletionPolicy: Delete/Retain
│   │   └── parameters
│   ├── 📷 VolumeSnapshot
│   │   ├── source: PVC or VolumeSnapshotContent
│   │   ├── volumeSnapshotClassName
│   │   └── readyToUse: true/false
│   └── 💾 VolumeSnapshotContent
│       ├── snapshotHandle (CSI snapshot ID)
│       ├── source: volumeHandle or snapshotHandle
│       └── volumeSnapshotRef
│
├── 🔄 Volume Operations
│   ├── 📈 Volume Expansion
│   │   ├── allowVolumeExpansion: true
│   │   ├── Edit PVC spec.resources.requests
│   │   ├── Controller Expansion (resize volume)
│   │   └── Node Expansion (resize filesystem)
│   ├── 🚀 Volume Mounting
│   │   ├── 🔗 Attach (Controller)
│   │   ├── 🗂️ Mount (Kubelet)
│   │   ├── 🔧 Format (if needed)
│   │   └── 📁 Bind Mount (into container)
│   └── 🧹 Volume Cleanup
│       ├── Unmount from container
│       ├── Unmount from node
│       ├── Detach from node
│       └── Delete volume (if policy allows)
│
└── 🔧 Advanced Storage Features
    ├── 📊 Volume Metrics
    │   ├── Capacity utilization
    │   ├── Available space
    │   ├── Inodes used/available
    │   └── Performance metrics
    ├── 🔒 Volume Security
    │   ├── 🔐 Encryption at rest
    │   ├── 🔑 Encryption in transit
    │   ├── 👤 fsGroup/fsGroupChangePolicy
    │   └── 🛡️ securityContext
    ├── 📍 Topology Awareness
    │   ├── Zone/Region constraints
    │   ├── Node affinity
    │   └── Pod spreading
    ├── 🔧 Volume Plugins (Legacy)
    │   ├── In-tree plugins (deprecated)
    │   ├── FlexVolume (deprecated)
    │   └── CSI migration
    └── 🎯 Use Case Patterns
        ├── 📊 Databases (StatefulSet + PVC)
        ├── 📝 Logs (hostPath, emptyDir)
        ├── ⚙️ Config (ConfigMap, Secret)
        ├── 💾 Shared Storage (NFS, CephFS)
        └── 🚀 Temporary (emptyDir, memory)
```

## Связи и отношения между Storage Resources:

### 🔗 Основные связи в хранилище:
```
StorageClass ──provisions──> PV ──binds to──> PVC ──mounts in──> Pod
    ↓                         ↓                  ↓               ↓
CSI Driver ──creates──> Cloud Volume ──attaches to──> Node ──mounts to──> Container
```

### 📊 Жизненный цикл хранилища:
```
1. 📋 PVC Creation
   User ──creates──> PVC ──requests──> StorageClass

2. 🏗️ Dynamic Provisioning
   StorageClass ──triggers──> CSI Driver ──creates──> PV
   
3. 🔗 Binding
   PV ──binds to──> PVC ──becomes──> Bound state

4. 📦 Pod Scheduling
   Pod ──references──> PVC ──requires──> Node with PV

5. 🔧 Volume Attachment
   CSI Controller ──attaches──> Volume to Node
   
6. 🗂️ Volume Mounting
   CSI Node ──stages & publishes──> Volume to Pod
   
7. 📁 Container Access
   Container ──accesses──> Mounted filesystem

8. 🧹 Cleanup
   Pod deletion ──triggers──> Volume unmount ──triggers──> Volume detach
```

### 🎯 Static vs Dynamic Provisioning:

#### Static Provisioning:
```
Admin ──pre-creates──> PV (with real storage)
User ──creates──> PVC ──binds to──> existing PV
```

#### Dynamic Provisioning:
```
User ──creates──> PVC ──references──> StorageClass
StorageClass ──uses──> Provisioner ──creates──> PV + real storage
PV ──auto-binds to──> PVC
```

### 🔌 CSI Driver Interactions:
```
Kubernetes API ──calls──> CSI Controller ──manages──> External Storage
CSI Node ──mounts volumes on──> Kubernetes Nodes
VolumeAttachment ──tracks──> attach/detach state
```

### 📸 Snapshot Workflows:
```
VolumeSnapshot ──creates──> VolumeSnapshotContent ──stores──> Actual Snapshot
VolumeSnapshotClass ──defines──> Snapshot parameters
Snapshot ──can restore to──> new PVC ──creates──> new PV
```

### 🔧 Volume Mount Process:
```
Pod ──references──> PVC ──bound to──> PV
Scheduler ──considers──> Volume topology
kubelet ──calls──> CSI Node ──stages──> Volume
CSI Node ──publishes──> Volume to Pod path
Container ──mounts──> Volume at mountPath
```

## 📋 Storage Resource Types Comparison:

| Resource | Scope | Purpose | Lifecycle | Management |
|----------|-------|---------|-----------|------------|
| **StorageClass** | Cluster | Define storage types | Long-lived | Admin |
| **PV** | Cluster | Represent storage | Varies by policy | Admin/Dynamic |
| **PVC** | Namespace | Request storage | Pod-bound | User |
| **ConfigMap Volume** | Namespace | Configuration data | Pod-bound | User |
| **Secret Volume** | Namespace | Sensitive data | Pod-bound | User |
| **EmptyDir** | Pod | Temporary storage | Pod lifecycle | Automatic |
| **HostPath** | Pod | Node local access | Pod lifecycle | User |
| **CSI Ephemeral** | Pod | Dynamic temporary | Pod lifecycle | CSI Driver |

## 🔐 Access Modes Matrix:

| Storage Type | RWO | ROX | RWX | Use Case |
|--------------|-----|-----|-----|----------|
| **Block Storage** | ✅ | ❌ | ❌ | Databases, Single writer |
| **Network FS** | ✅ | ✅ | ✅ | Shared data, Multi-reader |
| **Object Storage** | ❌ | ✅ | ❌ | Static content, Archives |
| **Local Storage** | ✅ | ❌ | ❌ | High performance, Node-bound |

## 🚀 Storage Performance Tiers:

### High Performance:
- Local NVMe/SSD
- Premium cloud storage
- High IOPS requirements

### Standard Performance:
- Standard cloud disks
- Network attached storage
- General purpose workloads

### Archive/Cold Storage:
- Infrequent access patterns
- Cost-optimized storage
- Backup and archival

## 🔧 Best Practices:

### 📊 Capacity Planning:
```
PVC Request ──should be──> Realistic estimate
PV Capacity ──should exceed──> PVC request
StorageClass ──should allow──> Volume expansion
Monitor ──disk usage──> Set alerts at 80%
```

### 🛡️ Security:
```
Secrets ──mounted as──> read-only volumes
FSGroup ──set for──> shared volume access
Encryption ──enabled for──> sensitive data
RBAC ──controls──> PVC creation
```

### 📈 Scalability:
```
Use ──dynamic provisioning──> for auto-scaling
Implement ──volume expansion──> for growth
Consider ──topology awareness──> for multi-zone
Monitor ──storage metrics──> for optimization
