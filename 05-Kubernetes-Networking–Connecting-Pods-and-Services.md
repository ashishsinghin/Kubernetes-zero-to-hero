### **Kubernetes Networking – Connecting Pods and Services**  

---

### **Introduction**  
Networking in Kubernetes is a critical component that enables communication between Pods, Services, and external clients. Kubernetes abstracts networking complexities and provides built-in mechanisms to ensure seamless communication across components.  

In this lesson, we will explore:  
- How Pods communicate within a cluster.  
- Different networking models in Kubernetes.  
- Services and how they enable external access.  
- DNS and networking policies for securing traffic.  

---

## **1. Kubernetes Networking Model**  
### **Key Networking Principles**  
Kubernetes follows a few fundamental networking rules:  
1. **Each Pod gets a unique IP address** – There is no need for NAT (Network Address Translation) between Pods.  
2. **All Pods can communicate with each other by default** – This allows seamless networking inside a cluster.  
3. **Containers within a Pod share the same network namespace** – They communicate over `localhost`.  

### **Types of Communication in Kubernetes**  
- **Pod-to-Pod Communication**: Direct communication between Pods inside the cluster.  
- **Pod-to-Service Communication**: Pods communicate with other services using Kubernetes Services.  
- **External-to-Service Communication**: External users or applications access services via Ingress or LoadBalancer.  

---

## **2. Services in Kubernetes**  
Since Pods are ephemeral, their IP addresses change frequently. Kubernetes Services provide a stable way to expose applications running in Pods.  

### **Types of Kubernetes Services**  
1. **ClusterIP (Default)** – Exposes the service inside the cluster.  
2. **NodePort** – Exposes the service on a static port on each Node.  
3. **LoadBalancer** – Provides an external load balancer to expose the service outside the cluster.  
4. **ExternalName** – Maps a service to an external domain name.  

### **Creating a Service**  
Here’s how to create a Service for an application running in a Pod:  

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: default
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```
This service routes traffic on port `80` to Pods running the application on port `8080`.  

---

## **3. DNS in Kubernetes**  
Kubernetes includes an internal **DNS service** that allows Pods to resolve each other using simple names instead of IP addresses.  

- **Format**: `<service-name>.<namespace>.svc.cluster.local`  
- Example: `my-service.default.svc.cluster.local`  

This eliminates the need to hardcode IPs in application configurations.  

---

## **4. Network Policies**  
By default, all Pods in a cluster can communicate freely. **Network Policies** restrict traffic between Pods based on rules.  

Example of a **Network Policy** allowing only specific Pods to communicate:  

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app-access
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
```
This policy allows only Pods with the label `role: frontend` to access `my-app` Pods.  

---

## **Conclusion**  
- **Pods communicate inside a cluster using unique IPs.**  
- **Kubernetes Services provide stable endpoints for applications.**  
- **DNS simplifies name resolution inside the cluster.**  
- **Network Policies enhance security by controlling traffic.**  

---

### **About the Instructor**
Ashish Singh specializes in backend development, DevOps, and cloud technologies. Follow for practical Kubernetes tutorials that help you build scalable and secure applications!

Follow me on [Social Media](https://ashishsingh.in/social-media/)
