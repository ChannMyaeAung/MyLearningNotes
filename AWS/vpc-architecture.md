# 🌐 AWS VPC Architecture Reference Guide

## 1. The Core Infrastructure (VPC & Subnets)
To deploy a standard web application with a secure database layer, we split the network into three distinct zones (subnets) across our VPC:

* **VPC:** The overall network boundary (e.g., `10.0.0.0/16`).
* **Public Subnet:** Used for resources that *must* face the public internet, like a Load Balancer or a public-facing web server (e.g., `10.0.0.0/24`).
* **Private Subnet 1 (Database):** Used for your primary AWS RDS database instance (e.g., `10.0.1.0/24`).
* **Private Subnet 2 (Database):** AWS RDS requires at least two private subnets in different Availability Zones (AZs) for high-availability backup and failover purposes (e.g., `10.0.2.0/24`).

---

## 2. Internet Gateway (IGW)
An Internet Gateway is the "front door" attached to your VPC. 
* Its entire purpose is to **allow communication between your VPC and the outside internet**. Without it, nothing inside your network can talk to the public web.

---

## 3. Route Tables (The Traffic Controllers)
A Route Table contains a set of rules (routes) that determine where network traffic is directed. Every subnet must be associated with a route table.

### 🏢 The Main Route Table
* Every VPC comes with a **Main Route Table** by default. 
* It handles local routing, allowing all subnets inside the VPC to talk to each other seamlessly. 
* **Best Practice:** Leave this alone and create custom route tables for your specific subnets.

### 🟢 The Public Route Table
* **Association:** Manually linked to your **Public Subnet**.
* **The "Everywhere" Route:** It contains a local route, plus a specific route pointing `0.0.0.0/0` (all internet traffic) directly to the **Internet Gateway**. This is what actually makes the public subnet "public."

### 🔴 The Private Route Table
* **Association:** Linked to your **two Private Subnets**.
* The private route table simply has a **Local Route only** (keeping it totally isolated from the internet). 

> 🔒 **How do we actually restrict access to the RDS database?**
> We use **Security Groups** (firewalls). You attach a Security Group to your AWS RDS database that explicitly says: *"Only allow incoming database traffic (e.g., port 5432 for PostgreSQL) if it is coming from the Public Subnet's Security Group."*

---

## 🗺️ Architectural Cheat Sheet (Mental Model)

| Component            | Connected To                     | CIDR Block Example | Internet Access?             |
| :------------------- | :------------------------------- | :----------------- | :--------------------------- |
| **VPC**              | Entire Project                   | `10.0.0.0/16`      | N/A                          |
| **Public Subnet**    | Public Route Table -> IGW        | `10.0.0.0/24`      | **Yes** (Inbound & Outbound) |
| **Private Subnet 1** | Private Route Table (Local only) | `10.0.1.0/24`      | **No** (Isolated)            |
| **Private Subnet 2** | Private Route Table (Local only) | `10.0.2.0/24`      | **No** (Isolated)            |

## 4. Step-by-Step Route Table Provisioning & Subnet Association

To implement this isolation in the AWS Console, custom route tables must be explicitly created and mapped to break away from the VPC's default fallback routing behavior.

### Step 1: Create the Custom Route Tables
Navigate to **VPC Dashboard -> Route Tables -> Create route table** and provision three distinct tables tied to your custom VPC (`re_vpc`) (I used re here because I was trying to learn and integrate AWS services to one of my web applications "HomeScout" which is like a real estate application:
1. `re_public-route-table-1` (Handles public frontend/ingress routing)
2. `re_private-route-table-1` (Handles isolated database routing for Availability Zone A)
3. `re_private-route-table-2` (Handles isolated database routing for Availability Zone B)

---

### Step 2: Establish Explicit Subnet Associations
By default, subnets are implicitly linked to the VPC's **Main Route Table**. To isolate them, you must configure **Explicit Subnet Associations**:

1. Select **`re_public-route-table-1`** -> Go to the **Actions** tab -> Click **Edit subnet associations** -> Check **`re_public-subnet-1`** -> Save.
2. Select **`re_private-route-table-1`** -> Go to the **Actions** tab -> Click **Edit subnet associations** -> Check **`re_private-subnet-1`** -> Save.
3. Select **`re_private-route-table-2`** -> Go to the **Actions** tab -> Click **Edit subnet associations** -> Check **`re_private-subnet-2`** -> Save.

---

### Step 3: Configure the Public Internet Route (The Final Gateway Link)
Creating a table named "public" does not make it public; you must add the rule that forwards traffic to your attached Internet Gateway (`re_internet-gateway`).

1. Select your **`re_public-route-table-1`**.
2. Click on the **Routes** tab at the bottom -> Click **Edit routes**.
3. Click **Add route** and configure the following mapping:
   * **Destination:** `0.0.0.0/0` (This represents all IPv4 traffic outside the local VPC network boundaries)
   * **Target:** Select **Internet Gateway** -> Choose your attached **`re_internet-gateway`**.
4. Click **Save changes**.

Your private route tables require no changes here. They will continue to hold only their default local VPC route (e.g., `10.0.0.0/16 -> local`), ensuring no packets can ever find a route out to or in from the public web.