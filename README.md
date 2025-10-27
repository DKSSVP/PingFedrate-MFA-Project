# 🔐 Secure Authentication Flow Using PingFederate with SAML

## 📘 Overview
This project demonstrates the implementation of a **secure, context-aware authentication system** using **PingFederate**.  
It showcases how **SAML 2.0**, **Composite Adapters**, **CIDR Selectors**, and **PingID MFA** can be integrated to build a dynamic authentication flow that adapts based on user location.

---

## 🧠 Key Features
- **Federated Authentication** using **SAML 2.0** (Service Provider configuration)
- **Conditional Multi-Factor Authentication (MFA)** using **PingID**
- **CIDR-based routing** for internal vs external network users
- **Composite Adapter** combining multiple authentication methods
- **Context-aware security** without compromising user experience

---

## ⚙️ Technologies Used
| Component | Description |
|------------|-------------|
| **PingFederate** | Identity and Access Management (IAM) platform |
| **SAML 2.0** | Security Assertion Markup Language for federated SSO |
| **PingID** | Multi-Factor Authentication service |
| **CIDR Selector** | Routes authentication based on IP range |
| **Composite Adapter** | Merges multiple adapters (HTML Form, HTTP Basic, PingID) |
| **Datastore (LDAP/JDBC)** | Backend user repository for credential validation |

---

## 🧩 System Architecture
**Flow Summary:**
1. **User Accesses SP (Service Provider)** → Redirects to PingFederate (IdP)
2. **CIDR Selector** checks the user’s IP:
   - **Internal IP** → Authenticate via **HTML/HTTP Basic Adapter**
   - **External IP** → Authenticate via **PingID MFA**
3. **SAML Assertion** sent to SP → User gets access

---

## 🔧 Step-by-Step Configuration

### 1. Configure Datastore
- Create a new data store under **Authentication → Data Stores**
- Provide connection details (hostname, port, base DN)
- Test and save configuration

### 2. Setup Password Credential Validator (PCV)
- Navigate to **Authentication → Credential Validators**
- Create new validator (e.g., *LDAP-PCV*) and link to datastore

### 3. Create Authentication Adapters
- Create **HTML Form Adapter** linked to PCV  
- Similarly create **HTTP Basic**, **PingID**, and **Composite Adapters**

### 4. Define CIDR Authentication Selector
- Create a **CIDR Selector** with IP ranges (e.g., 192.168.1.0/24)  
- Save the selector for policy routing

### 5. Build Authentication Policy
- Internal IPs → No MFA  
- External IPs → Enforce **PingID MFA**

### 6. Setup SAML Service Provider Connection
- Add new **SP connection** using **SAML 2.0**
- Upload metadata, assign authentication policy, and save

### 7. Test Authentication Flow
- Login via **internal IP** → Direct access (no MFA)
- Login via **external IP** → MFA triggered via PingID
- Verify results in PingFederate logs

---

## 🧾 Results & Observations
- MFA enforced only for external users (CIDR worked accurately)
- Internal users had seamless access without MFA
- Composite Adapter provided flexibility for multiple login methods
- SAML assertions successfully passed to the Service Provider

---

## 🏁 Conclusion
This mini project successfully demonstrates a **secure, policy-driven authentication architecture** using PingFederate.  
It achieves:
- **Dynamic, IP-based security enforcement**  
- **Improved usability** for trusted users  
- **Robust MFA protection** for external access  

This implementation represents a scalable and modern **IAM (Identity and Access Management)** practice suitable for enterprise-grade security.

---

## 📚 References
- [PingFederate Documentation](https://docs.pingidentity.com/)
- [PingID Integration Guide](https://docs.pingidentity.com/)
- [SAML 2.0 Technical Overview (OASIS)](https://docs.oasis-open.org/security/saml/)
- Internal PingFederate Admin Console
