# AWS Network Isolation: Public vs. Private Subnets

The rule of thumb for cloud security is simple: **Minimize the attack surface.** If a resource doesn’t absolutely need a direct connection to the public internet, hide it. 

In a standard web app setup, this means putting the API/backend servers in a public subnet and the database in a private subnet.

---

## The Subnets

* **Public Subnet:** Connected directly to an Internet Gateway (IGW). Resources here get public IPs and can talk directly to the outside world.
* **Private Subnet:** Completely cut off from the internet. Resources only have internal VPC IPs (e.g., `10.0.x.x`) and can only talk to other things inside the same network.

---

## Why EC2 Is Public, but RDS Is Private

### 1. EC2 (The Gatehouse)
The backend code (Node, FastAPI, Go, etc.) *has* to accept incoming HTTP traffic from users or frontends (like Vercel). Because it's customer-facing, it lives in the public subnet or sits behind an internet-facing Load Balancer. It acts as a shield, inspecting incoming requests before passing anything down the line.

### 2. RDS (The Vault)
The database holds user data and credentials. If you put RDS in a public subnet, it gets a public IP. Even with tight firewall rules (Security Groups), exposing database ports (`5432` for Postgres, `27017` for Mongo) to the open web invites constant automated brute-force attacks, scans, and DDoS attempts.

By putting RDS in a private subnet:
* It has no public IP. It is physically impossible to route internet traffic to it.
* Only the EC2 instances inside the VPC can talk to it using internal private networking.

---

## The Architecture Flow

```text
[ Public Internet ]
       │
       ▼ (HTTP Requests)
┌───────────────────────────────────────────┐
│ AWS VPC                                   │
│                                           │
│  Public Subnet:                           │
│  [ EC2 Backend Server / Load Balancer ]   │
│         │                                 │
│         ▼ (Internal Traffic Only)         │
│                                           │
│  Private Subnet:                          │
│  [ RDS Database (No Public IP) ]          │
│                                           │
└───────────────────────────────────────────┘