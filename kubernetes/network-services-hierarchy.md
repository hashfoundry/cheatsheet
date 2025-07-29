# Иерархия Network Services в Kubernetes

```
🌐 Kubernetes Network Services Ecosystem
│
├── 🏷️ Services (Service Discovery & Load Balancing)
│   ├── 🔵 ClusterIP (Default)
│   │   ├── 📋 Purpose: Internal cluster communication
│   │   ├── 🌐 IP Range: Cluster CIDR (10.96.0.0/12)
│   │   ├── 🎯 Target: Pods via selector
│   │   ├── 🔌 Port Mapping: port → targetPort
│   │   ├── 📊 Session Affinity: ClientIP/None
│   │   └── 🔄 Load Balancing: Round-robin/Random
│   │
│   ├── 🔶 NodePort
│   │   ├── 📋 Purpose: External access via node IP
│   │   ├── 🌐 Port Range: 30000-32767 (default)
│   │   ├── 🎯 Target: ClusterIP + Node ports
│   │   ├── 🔌 Access: <NodeIP>:<NodePort>
│   │   ├── 📡 External Traffic Policy: Cluster/Local
│   │   └── 🔄 Automatically creates ClusterIP
│   │
│   ├── ⚖️ LoadBalancer
│   │   ├── 📋 Purpose: Cloud provider load balancer
│   │   ├── 🌐 External IP: Provisioned by cloud
│   │   ├── 🎯 Target: NodePort + External LB
│   │   ├── 🔌 Health Checks: Configure via annotations
│   │   ├── 📡 Load Balancer Class: Specify provider
│   │   └── 🔄 Inherits NodePort functionality
│   │
│   ├── 🔗 ExternalName
│   │   ├── 📋 Purpose: DNS CNAME mapping
│   │   ├── 🌐 Maps to: External FQDN
│   │   ├── 🎯 No selectors or endpoints
│   │   ├── 🔌 DNS Resolution: Returns CNAME
│   │   └── 📊 Use case: External service aliases
│   │
│   └── 👤 Headless Service (ClusterIP: None)
│       ├── 📋 Purpose: Direct Pod access
│       ├── 🌐 DNS: Returns Pod IPs directly
│       ├── 🎯 Target: StatefulSet Pods
│       ├── 🔌 No load balancing/proxy
│       └── 📊 Use case: Databases, StatefulSets
│
├── 📍 Endpoints & EndpointSlices
│   ├── 📋 Endpoints (Legacy)
│   │   ├── 🎯 Pod IP addresses + ports
│   │   ├── 🔄 Updated by Endpoints Controller
│   │   ├── 📊 One per Service
│   │   ├── 🚀 Performance: Limited scalability
│   │   └── 🔧 Manual management possible
│   │
│   └── 📊 EndpointSlices (Modern)
│       ├── 🎯 Scalable endpoint management
│       ├── 🔄 Updated by EndpointSlice Controller
│       ├── 📊 Multiple slices per Service
│       ├── 🚀 Performance: Better for large clusters
│       ├── 🔧 Topology awareness
│       ├── 📍 Zone/Region information
│       └── 🏷️ Conditions: ready, serving, terminating
│
├── 🚪 Ingress (HTTP/HTTPS Routing)
│   ├── 📋 Ingress Resources
│   │   ├── 🌐 Host-based routing: example.com
│   │   ├── 🛤️ Path-based routing: /api, /web
│   │   ├── 🔒 TLS termination: certificates
│   │   ├── 🎯 Backend Services: service + port
│   │   ├── 📊 Rules: host + path → service
│   │   ├── 🔧 Annotations: controller-specific
│   │   └── 📍 Default backend: fallback service
│   │
│   ├── 🎮 Ingress Controllers
│   │   ├── 🌍 NGINX Ingress Controller
│   │   │   ├── Load balancing algorithms
│   │   │   ├── Rate limiting & auth
│   │   │   ├── SSL/TLS termination
│   │   │   └── WebSocket support
│   │   ├── 🔷 Traefik
│   │   │   ├── Automatic service discovery
│   │   │   ├── Let's Encrypt integration
│   │   │   ├── Circuit breakers
│   │   │   └── Metrics & tracing
│   │   ├── 🏠 HAProxy Ingress
│   │   ├── 🌊 Istio Gateway
│   │   ├── ☁️ Cloud Provider Controllers
│   │   │   ├── AWS ALB Controller
│   │   │   ├── GCP GCE Controller
│   │   │   └── Azure Application Gateway
│   │   └── 🎯 Custom Controllers
│   │
│   └── 📋 IngressClass
│       ├── 🎮 Controller specification
│       ├── ⚙️ Parameters & configuration
│       ├── 🏷️ Default class annotation
│       └── 🔧 Multi-controller support
│
├── 🛡️ Network Policies (Micro-segmentation)
│   ├── 📋 Policy Types
│   │   ├── 📥 Ingress Rules (incoming traffic)
│   │   │   ├── 🎯 From: namespaceSelector, podSelector
│   │   │   ├── 🔌 Ports: protocol + port ranges
│   │   │   ├── 🌐 Source IPs: CIDR blocks
│   │   │   └── 🚫 Default: Deny all ingress
│   │   └── 📤 Egress Rules (outgoing traffic)
│   │       ├── 🎯 To: namespaceSelector, podSelector
│   │       ├── 🔌 Ports: protocol + port ranges
│   │       ├── 🌐 Destination IPs: CIDR blocks
│   │       └── 🚫 Default: Allow all egress
│   │
│   ├── 🎯 Pod Selection
│   │   ├── 🏷️ podSelector: label-based
│   │   ├── 📁 namespaceSelector: namespace-based
│   │   ├── 🔗 Combination: AND logic
│   │   └── 🌐 Empty selector: all pods
│   │
│   ├── 🔧 Implementation
│   │   ├── 🌐 CNI Plugin dependent
│   │   ├── 🛡️ Calico: iptables/eBPF
│   │   ├── 🔷 Cilium: eBPF-based
│   │   ├── 🌊 Weave: iptables-based
│   │   └── 📊 Monitoring: policy violations
│   │
│   └── 💡 Use Cases
│       ├── 🏢 Multi-tenant isolation
│       ├── 🛡️ PCI DSS compliance
│       ├── 📊 Database access control
│       └── 🚫 Zero-trust networking
│
├── 🌍 DNS (Service Discovery)
│   ├── 🔍 CoreDNS (Default)
│   │   ├── 📋 Cluster DNS provider
│   │   ├── ⚙️ Configurable via ConfigMap
│   │   ├── 🔌 Plugins: kubernetes, forward, cache
│   │   ├── 🎯 Service discovery: svc.cluster.local
│   │   ├── 📊 Pod DNS: pod-ip.ns.pod.cluster.local
│   │   ├── 🔄 Conditional forwarding: external DNS
│   │   └── 📈 Metrics & logging
│   │
│   ├── 📋 DNS Resolution
│   │   ├── 🏷️ Service: <service>.<namespace>.svc.cluster.local
│   │   ├── 👤 Headless: <pod>.<service>.<namespace>.svc.cluster.local
│   │   ├── 📦 Pod: <pod-ip>.<namespace>.pod.cluster.local
│   │   ├── 🌐 External: Forward to upstream DNS
│   │   └── 🔄 Search domains: namespace-aware
│   │
│   ├── 🎯 DNS Policies
│   │   ├── 🔵 ClusterFirst (default): Cluster DNS first
│   │   ├── 🌐 Default: Node DNS configuration
│   │   ├── 🚫 None: No DNS configuration
│   │   └── 🔄 ClusterFirstWithHostNet: Host network pods
│   │
│   └── 🔧 DNS Configuration
│       ├── ⚙️ dnsConfig: custom DNS settings
│       ├── 📍 nameservers: custom DNS servers
│       ├── 🔍 searches: additional search domains
│       └── ⚙️ options: resolver options
│
├── 🌐 Container Network Interface (CNI)
│   ├── 📋 CNI Plugins
│   │   ├── 🛡️ Calico
│   │   │   ├── 📊 Features: NetworkPolicy, BGP, eBPF
│   │   │   ├── 🌐 IP Management: IPAM plugins
│   │   │   ├── 🔒 Security: Wireguard encryption
│   │   │   └── 📈 Performance: High throughput
│   │   ├── 🔷 Cilium
│   │   │   ├── 📊 Features: eBPF-based, Service Mesh
│   │   │   ├── 🛡️ Security: Identity-based policies
│   │   │   ├── 📡 Observability: Hubble
│   │   │   └── 🚀 Performance: Kernel bypass
│   │   ├── 🌊 Weave Net
│   │   │   ├── 📊 Features: Simple setup, encryption
│   │   │   ├── 🌐 Overlay network: VXLAN
│   │   │   ├── 🔍 Service discovery: built-in
│   │   │   └── 📊 Monitoring: WeaveScope
│   │   ├── 🔶 Flannel
│   │   │   ├── 📊 Features: Simple overlay networking
│   │   │   ├── 🌐 Backend: VXLAN, host-gw, UDP
│   │   │   ├── 🚀 Performance: Low latency
│   │   │   └── 📋 Use case: Basic networking
│   │   └── ☁️ Cloud Provider CNIs
│   │       ├── 🌟 AWS VPC CNI
│   │       ├── 🔷 Azure CNI
│   │       └── 🌍 GCP Alias IP
│   │
│   ├── 🔧 CNI Configuration
│   │   ├── 📋 /etc/cni/net.d/: config files
│   │   ├── 🔌 /opt/cni/bin/: binary plugins
│   │   ├── ⚙️ Network configuration: JSON format
│   │   ├── 🌐 IPAM: IP address management
│   │   └── 🔗 Chaining: multiple plugins
│   │
│   └── 📊 CNI Operations
│       ├── 🔌 ADD: Create network interface
│       ├── 🗑️ DEL: Delete network interface
│       ├── ✅ CHECK: Verify network interface
│       └── 📋 VERSION: Plugin version info
│
├── ⚖️ kube-proxy (Service Implementation)
│   ├── 📋 Proxy Modes
│   │   ├── 📊 iptables (default)
│   │   │   ├── 🔧 Implementation: iptables rules
│   │   │   ├── 🎯 Load balancing: Random selection
│   │   │   ├── 🚀 Performance: Good for small clusters
│   │   │   └── 📊 Scalability: O(n) rule complexity
│   │   ├── 🚀 IPVS (IP Virtual Server)
│   │   │   ├── 🔧 Implementation: Kernel load balancer
│   │   │   ├── 🎯 Load balancing: Multiple algorithms
│   │   │   ├── 🚀 Performance: Better for large clusters
│   │   │   └── 📊 Scalability: O(1) lookup
│   │   ├── 🔗 userspace (deprecated)
│   │   └── 🪟 kernelspace (Windows)
│   │
│   ├── ⚖️ Load Balancing Algorithms (IPVS)
│   │   ├── 🔄 Round Robin (rr)
│   │   ├── ⚖️ Weighted Round Robin (wrr)
│   │   ├── 🔗 Least Connections (lc)
│   │   ├── ⚖️ Weighted Least Connections (wlc)
│   │   ├── 🎯 Source Hashing (sh)
│   │   └── 🎯 Destination Hashing (dh)
│   │
│   ├── 🔧 Configuration
│   │   ├── 📋 --proxy-mode: iptables/ipvs
│   │   ├── 🌐 --cluster-cidr: Pod network CIDR
│   │   ├── ⚙️ --config: Configuration file
│   │   └── 📊 --metrics-bind-address: Metrics endpoint
│   │
│   └── 📊 Responsibilities
│       ├── 🎯 Service load balancing
│       ├── 🔄 Endpoint updates
│       ├── 📡 Session affinity
│       └── 📊 Service metrics
│
├── 🕸️ Service Mesh (Advanced Networking)
│   ├── 🌊 Istio
│   │   ├── 🎮 Control Plane
│   │   │   ├── 📋 Pilot: Traffic management
│   │   │   ├── 🔒 Citadel: Security & certificates
│   │   │   ├── 🌐 Galley: Configuration validation
│   │   │   └── 📊 Telemetry: Metrics collection
│   │   ├── 📊 Data Plane
│   │   │   ├── 🔌 Envoy Proxy: Sidecar containers
│   │   │   ├── 🔄 Load balancing: Advanced algorithms
│   │   │   ├── 🔒 mTLS: Automatic encryption
│   │   │   └── 📡 Observability: Distributed tracing
│   │   ├── 🚪 Gateway
│   │   │   ├── 🌐 Ingress Gateway: External traffic
│   │   │   ├── 📤 Egress Gateway: Outbound traffic
│   │   │   ├── 🔒 TLS termination: Certificate management
│   │   │   └── 🎯 Traffic routing: Advanced rules
│   │   └── 🛡️ Security Features
│   │       ├── 🔐 Authentication: JWT validation
│   │       ├── 🚫 Authorization: RBAC policies
│   │       ├── 📊 Audit logging: Security events
│   │       └── 🔒 Zero-trust: Default deny
│   │
│   ├── 🔗 Linkerd
│   │   ├── 🎮 Control Plane: Lightweight design
│   │   ├── 📊 Data Plane: Rust-based proxy
│   │   ├── 🔒 Security: Automatic mTLS
│   │   ├── 📡 Observability: Built-in metrics
│   │   └── 🚀 Performance: Low latency overhead
│   │
│   ├── 🌐 Consul Connect
│   │   ├── 🎮 Control Plane: HashiCorp Consul
│   │   ├── 📊 Data Plane: Envoy integration
│   │   ├── 🔍 Service Discovery: Native Consul
│   │   └── 🔒 Security: Intentions-based policies
│   │
│   └── 📊 Service Mesh Features
│       ├── 🔄 Traffic Management
│       │   ├── 🎯 Load balancing strategies
│       │   ├── 🔄 Circuit breakers
│       │   ├── ⏱️ Timeouts & retries
│       │   └── 🚦 Traffic splitting (A/B testing)
│       ├── 🔒 Security
│       │   ├── 🔐 Mutual TLS (mTLS)
│       │   ├── 🛡️ Authorization policies
│       │   ├── 📊 Audit & compliance
│       │   └── 🔑 Certificate management
│       └── 📡 Observability
│           ├── 📊 Metrics: Prometheus/Grafana
│           ├── 📍 Distributed tracing: Jaeger
│           ├── 📝 Logging: Centralized logs
│           └── 🔍 Service topology: Visualization
│
└── 🔧 Advanced Network Features
    ├── 📊 Network Observability
    │   ├── 🔍 Network Flow Analysis
    │   │   ├── 📡 Hubble (Cilium): eBPF-based
    │   │   ├── 🌊 WeaveScope: Visual networking
    │   │   ├── 📊 Falco: Runtime security
    │   │   └── 🔍 Sysdig Inspect: Deep visibility
    │   ├── 📈 Network Metrics
    │   │   ├── 🌐 Pod-to-Pod latency
    │   │   ├── 📊 Service response times
    │   │   ├── 🔄 Connection counts
    │   │   └── 📡 Bandwidth utilization
    │   └── 📋 Troubleshooting Tools
    │       ├── 🔍 kubectl port-forward
    │       ├── 🌐 Network Policy testing
    │       ├── 📊 DNS resolution debugging
    │       └── 🔧 Connectivity validation
    │
    ├── 🌍 Multi-Cluster Networking
    │   ├── 🔗 Cluster Mesh (Cilium)
    │   │   ├── 🌐 Cross-cluster Pod communication
    │   │   ├── 🎯 Global service discovery
    │   │   ├── 🛡️ Unified network policies
    │   │   └── 📊 Multi-cluster observability
    │   ├── 🌊 Istio Multi-Cluster
    │   │   ├── 🔗 Cross-cluster service mesh
    │   │   ├── 🎯 Global load balancing
    │   │   ├── 🔒 Unified security policies
    │   │   └── 📡 Cross-cluster telemetry
    │   └── 🔌 Submariner
    │       ├── 🌐 L3 connectivity between clusters
    │       ├── 🔍 Cross-cluster service discovery
    │       └── 🛡️ Encrypted tunnels
    │
    ├── 🔒 Network Security
    │   ├── 🛡️ Zero Trust Networking
    │   │   ├── 🚫 Default deny policies
    │   │   ├── 🔐 Identity-based access
    │   │   ├── 📊 Continuous monitoring
    │   │   └── 🔍 Audit & compliance
    │   ├── 🔐 Encryption
    │   │   ├── 🔒 Pod-to-Pod: WireGuard/IPSec
    │   │   ├── 🌐 In-transit: TLS/mTLS
    │   │   ├── 📊 At-rest: Storage encryption
    │   │   └── 🔑 Key management: Vault integration
    │   └── 🚫 Runtime Protection
    │       ├── 📊 Anomaly detection
    │       ├── 🛡️ DDoS protection
    │       ├── 🔍 Intrusion detection
    │       └── 📋 Threat intelligence
    │
    └── 🚀 Performance Optimization
        ├── 📊 Network Performance
        │   ├── 🔧 SR-IOV: Hardware acceleration
        │   ├── 🚀 DPDK: Kernel bypass
        │   ├── 🌐 eBPF: Efficient packet processing
        │   └── 📡 CPU pinning: Network workloads
        ├── 🎯 Load Balancing Optimization
        │   ├── ⚖️ Session affinity tuning
        │   ├── 🔄 Connection pooling
        │   ├── 📊 Health check optimization
        │   └── 🎯 Geographic load balancing
        └── 📈 Scaling Considerations
            ├── 🌐 DNS caching strategies
            ├── 📊 Service mesh overhead
            ├── 🔧 Network policy efficiency
            └── 📡 Monitoring & alerting setup
```

## Связи и отношения между Network Services:

### 🔗 Основной поток сетевого трафика:
```
External Client ──> Ingress ──> Service ──> Endpoints ──> Pod
                        ↓           ↓           ↓         ↓
                   IngressController  kube-proxy  EndpointSlice  Container
```

### 📊 Service Discovery Chain:
```
Pod ──DNS query──> CoreDNS ──resolves──> Service ──selects──> Pod IPs
 ↓                    ↓                      ↓                ↓
Container     Cluster DNS Config    Service Selector    Endpoint
```

### 🛡️ Network Policy Enforcement:
```
Pod A ──packet──> CNI Plugin ──applies──> NetworkPolicy ──allows/denies──> Pod B
  ↓                   ↓                         ↓                           ↓
iptables/eBPF    Policy Engine         Security Rules              Target Pod
```

### 🎯 Load Balancing Flow:
```
Service Request ──> kube-proxy ──load balances──> Endpoint ──routes to──> Pod
      ↓                ↓                              ↓                    ↓
   Client           iptables/IPVS              Healthy Pods           Application
```

### 🔌 Service Types Hierarchy:
```
ExternalName ──DNS CNAME mapping
     ↓
LoadBalancer ──creates──> NodePort ──creates──> ClusterIP ──selects──> Pods
     ↓                       ↓                     ↓              ↓
Cloud LB              External Access        Internal Access    Application
```

### 🌐 CNI Integration:
```
kubelet ──calls──> CNI Plugin ──configures──> Pod Network ──connects to──> Node Network
   ↓                   ↓                          ↓                          ↓
Pod Creation      Network Setup             Pod IP Assignment        Cluster Network
```

### 🕸️ Service Mesh Data Flow:
```
Pod A ──> Envoy Sidecar ──mTLS──> Envoy Sidecar ──> Pod B
  ↓           ↓                         ↓             ↓
App          Traffic Mgmt          Traffic Mgmt     App
             Observability         Security         
             Policy Enforcement    Load Balancing   
```

## 📋 Network Services Comparison:

| Service Type | Scope | Access Method | Use Case | Load Balancing |
|--------------|-------|---------------|----------|----------------|
| **ClusterIP** | Internal | Service IP | Internal communication | Yes |
| **NodePort** | External | Node IP:Port | Development/Testing | Yes |
| **LoadBalancer** | External | Cloud LB | Production external access | Yes |
| **ExternalName** | External | DNS CNAME | External service proxy | No |
| **Headless** | Internal | Pod IPs directly | StatefulSets/Databases | No |

## 🔧 CNI Plugin Comparison:

| CNI Plugin | Type | Features | Performance | Use Case |
|------------|------|----------|-------------|----------|
| **Calico** | L3 | NetworkPolicy, BGP | High | Security-focused |
| **Cilium** | eBPF | Service Mesh, Observability | Highest | Modern cloud-native |
| **Flannel** | Overlay | Simple setup | Medium | Basic networking |
| **Weave** | Overlay | Encryption, Discovery | Medium | Easy deployment |
| **Cloud CNI** | Native | Cloud integration | High | Cloud-specific |

## 🛡️ Network Security Layers:

### 1. Cluster Level:
- Network segmentation
- Firewall rules
- VPC/Network isolation

### 2. Namespace Level:
- Network Policies
- Resource isolation
- RBAC integration

### 3. Pod Level:
- Security contexts
- Container network isolation
- Runtime security

### 4. Application Level:
- Service mesh policies
- mTLS encryption
- Authentication/Authorization

## 📈 Performance Optimization Guidelines:

### 🎯 Service Performance:
```
Use ClusterIP ──for internal communication
Use LoadBalancer ──for external production traffic
Configure ──session affinity for stateful apps
Monitor ──service latency and errors
```

### 🌐 DNS Optimization:
```
Configure ──DNS caching policies
Use ──shorter DNS search paths
Monitor ──DNS resolution times
Optimize ──CoreDNS configuration
```

### 🔧 CNI Performance:
```
Choose ──appropriate CNI for workload
Configure ──network policies efficiently
Monitor ──network bandwidth usage
Use ──SR-IOV for high-performance workloads
```

### 📊 Observability Best Practices:
```
Implement ──distributed tracing
Monitor ──service-to-service communication
Set up ──network policy compliance alerts
Track ──service mesh overhead metrics
