# Kubernetes-Services-Service-Discovery-Load-Balancing

Kubernetes Services | Service Discovery | Load Balancing
Why Do We Need Kubernetes Services?

In Kubernetes:

Pods are temporary

Pod IP addresses change when Pods restart

Directly using Pod IPs is not reliable

👉 To solve this problem, Kubernetes provides Services.

What is a Kubernetes Service?

A Kubernetes Service is an abstraction that provides:

A stable IP address

A DNS name

A way to access one or more Pods

Even if Pods restart or change IPs, the Service remains the same.

Example

Pods:

Pod-1 → 10.244.1.5
Pod-2 → 10.244.1.6
Pod-3 → 10.244.1.7


Service:

my-app-service → 10.96.120.10


Clients communicate with the Service, not individual Pods.

What is Service Discovery?

Service Discovery means Kubernetes automatically finds the correct Pods and connects applications to them.

Kubernetes supports two service discovery methods.

1. Environment Variable Based (Legacy)

When a Pod starts, Kubernetes injects environment variables:

MY_APP_SERVICE_HOST
MY_APP_SERVICE_PORT


⚠️ This method is rarely used today.

2. DNS Based Service Discovery (Most Common)

Kubernetes uses CoreDNS.

Each Service gets a DNS name:

my-app-service.default.svc.cluster.local


Inside the cluster, applications can simply use:

curl http://my-app-service


Kubernetes automatically resolves it to the correct Pods.

Load Balancing in Kubernetes

When a Service targets multiple Pods, Kubernetes automatically performs load balancing.

How it works:

Incoming traffic hits the Service

Service forwards requests to Pods

Requests are distributed evenly (round-robin)

Client → Service → Pod-1
                 → Pod-2
                 → Pod-3


No manual load balancer configuration is required.

Types of Kubernetes Services
1. ClusterIP (Default)

Accessible only inside the cluster

Used for internal communication

type: ClusterIP


Use cases:

Backend services

Databases

Internal APIs

2. NodePort

Exposes the Service on a static port of each node

Accessible using:

<NodeIP>:<NodePort>

type: NodePort


⚠️ Not recommended for production use.

3. LoadBalancer

Creates an external load balancer (cloud environments)

Supported by AWS, Azure, GCP

type: LoadBalancer


Examples:

AWS → ELB

Azure → Azure Load Balancer

GCP → Cloud Load Balancer

4. ExternalName

Maps a Service to an external DNS name

type: ExternalName


Example:

my-db-service → rds.amazonaws.com

Real-Life Analogy

Pods → Taxis

Service → Ride-sharing app (Uber/Ola)

Service Discovery → App finds available taxis automatically

Load Balancing → Requests distributed across multiple taxis

Interview-Ready One-Liners

What is a Kubernetes Service?
A Service provides a stable endpoint to access Pods.

What is Service Discovery?
Kubernetes automatically discovers and connects services using DNS.

How does load balancing work in Kubernetes?
Traffic is distributed evenly across multiple Pods using Services.

Simple Architecture Flow
User
 ↓
Service (Stable IP / DNS)
 ↓
Pod-1   Pod-2   Pod-3
