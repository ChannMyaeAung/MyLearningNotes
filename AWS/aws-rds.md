# AWS RDS: Core Concepts & Sandbox/Free Tier Realities

Amazon RDS (Relational Database Service) is a managed database service. 

## 1. Core Architectural Decisions in RDS

When creating an RDS instance, three main settings dictate cost, performance, and reliability:

### A. Deployment Options: Single-AZ vs. Multi-AZ
* **Single-AZ (Development/Sandbox):** Your database runs on a single physical machine in one Availability Zone (datacenter). If that datacenter loses power or hardware fails, your database goes down. 
* **Multi-AZ (Production):** AWS automatically provisions a synchronous "standby" replica of your database in a completely different Availability Zone. 
    * If the primary database crashes, AWS automatically flips the DNS traffic to the standby instance (**Failover**). Your application experience zero code changes and minimal downtime.

### B. Storage Scaling: Provisioned vs. Storage Autoscaling
* You start by allocating a baseline disk size (e.g., 20 GB gp3 storage).
* **Storage Autoscaling:** You should always enable this. If your database suddenly gets hit with a massive influx of data and hits 90% disk capacity, AWS will automatically scale up the storage volume on the fly without causing downtime.
  * However if we are practicing and learning AWS RDS, we should disable this.


### C. Backup & Retention
* RDS takes automated snapshots of your data daily and captures transaction logs continuously.
* This enables **Point-in-Time Recovery (PITR)**. If a bad migration or a rogue script wipes your data at 4:15 PM, you can tell RDS to spin up a brand new database clone exactly as it looked at 4:14 PM.

---

## 2. The Production Reality Check (Interview Knowledge)

In production, you never run a massive web application directly against a standard relational database instance without considering the connection limits.

* **The Bottleneck:** Relational databases like Postgres have a hard limit on concurrent connections because every connection consumes server memory. If a horizontally scaled API tier throws 500 simultaneous connections at a mid-sized RDS instance, the database will run out of memory and reject new requests.
* **The Solution:** In the real world, architects place an **RDS Proxy** or a connection pooler (like PgBouncer) between the EC2 instances and the database. The proxy pools and reuses database connections, allowing thousands of application connections to share a fraction of actual database connections.

---

## Summary Cheat Sheet for Setup & Interviews

* **Rule of Thumb:** Never check the "Publicly Accessible" box when creating an RDS instance for an app backend. Always keep it private and route traffic securely through your VPC via Security Groups.

---

## RDS Setup Instructions

1. Inside the AWS Console, search for AWS RDS and click Aurora and AWS.

2. Go to the databases tab on the left menu and click "Create Database" and choose full configuration.

3. After that, we can chose the engine type ("Postgres" e.g.).

4. Choose the creation method "Full configuration" for full control over everything and keep the cost as minimal and necessary as possible.

5. We can choose Templates as necessary for our use cases.

6. Then, we can give appropriate name for DB instance identifier inside "Settings" (re-rds for example).

7. Then we can choose either self-managed or Managed in AWS Secrets Manager which is more secure.

8. Then for storage, we should enable storage autoscaling. Only disable if we are testing or learning AWS RDS.

9. Then we have to carefully select the VPC we want because we cannot change the VPC after the database is created.

10. For VPC security group (firewall), we can choose to create new.

11. And then we give a meaningful name to the VPC security group name like "re_rds-sg" for example and then choose the availability zone.

12. Then in the "Monitoring" section, if we don't want to get charged, we can disable Performance Insights or for production it's better to enable it.

13. After that, we go down to "Additional Configuration", we give meaning name to "Initial database name" like "realestate" for example.

14. After that we leave everything to default and click "create database".

15. And then as it is creating, we can go to the database link, Find "Security group rules" and click the link there. We will get redirected to a new tab.

16. In the new Tab, Navigate to the RDS Security Group, open the Inbound rules tab, and click Edit inbound rules.

17. Action: Add a new rule with Type: PostgreSQL and set the Source to the Security Group ID of the EC2 instance (e.g., re_ec2-sgxxxxxxx).

    1. Why do we do this?
       Think of the Security Group as a strict bouncer for your database. By default, it denies all incoming traffic.

       ​	We add this rule because an EC2 instance and an RDS database exist as two separate entities in AWS; even if they live in the same account, the database doesn't "know" or trust the EC2 instance yet. By setting the Source to the EC2's Security Group ID, we aren't just whitelisting an IP address (which can change if the EC2 restarts); we are essentially giving the EC2 a "security badge."

       ​	This allows the database to instantly recognize any request coming from an instance wearing that badge, keeping the connection secure within AWS's private network while avoiding the headaches of managing fluctuating public IP addresses.

18. Next step: Granting EC2 Outbound Access to RDS.

19. Go back to the EC2 console, navigate to Instances, and click on the instance you are connecting to the database.

20. Select the Security tab below the instance details, click on your EC2's security group, go to Outbound rules, and click Edit outbound rules.

21. Click Add Rule, set the Type to PostgreSQL (port 5432), set the Source/Destination to the RDS security group (e.g., re_rds-sgxxxxx), and hit Save rules.

22. Why do we do this?

    - While inbound rules control who is allowed to enter a resource, outbound rules control what a resource is allowed to leave and talk to.
    - By default, a fresh EC2 security group usually has a blanket outbound rule allowing it to talk to "Anywhere" (0.0.0.0/0). However, in production environments or locked-down security setups, you don't want your application server reaching out to random destinations on the internet.
    - By explicitly creating an outbound rule targeting the RDS security group, you are telling the EC2 instance: "You are officially allowed to initiate a connection out to this specific database container on port 5432." It completes the handshake and the EC2 explicitly knows where it's allowed to send data, and the RDS explicitly knows who it's allowed to accept data from. Without both sides agreeing to talk, the connection fails.

23. After that, we go back to AWS RDS, and go to Databases.

24. Choose Endpoints for "Connect using" and copy the endpoint under "Endpoint & port".

25. Then paste inside any text editor in this format. (DATABASE_URL="postgresql://postgres:[database-password]@[endpoint-copied-from-aws-rds]:5432/[dbname-copied-from-aws-rds]?schema=public").

    1. The database name can be copied from the Configuration Tabs called "DB name" inside "Databases" tab inside AWS RDS Console.

26. And then we copied the DATABASE_URL and go back to the EC2 Virtual Connect.

27. Then we type the following commands:

    ```bash
    pm2 delete all
    nano .env
    ```

28. And then we can paste the DATABASE_URL and then save it.

29. Then if we are using prisma:

    ```bash
    pnpm run prisma:generate
    or
    npm run prisma:generate
    ```

30. After that

    ```bash
    pnpm exec prisma migrate deploy # if pnpm 
    npx prisma migrate dev --name init # if npm
    ```

31. And then we can run the seeding like this

    ```bash
    pnpm run seed # if pnpm
    npm run seed # if npm
    ```

32. After that we run

    ```bash
    pnpm run build
    ```

33. And we can start our pm2 again:

    ```bash
    pm2 start ecosystem.config.cjs
    ```

34. If the pm2 shows the application is running successfully, we can copy the public IPv4 address from the EC2 Console, and go to `http://[IPv4Addr-From-EC2]` and check all endpoints to make sure they are all up and running and working successfully.


# Debugging AWS RDS Connection Timeouts with Prisma (P1001)

## What Happened?

I was deploying my backend to an **AWS EC2 instance (Amazon Linux 2023)** and trying to sync my database schema to **AWS RDS (PostgreSQL)** using Prisma. 

Even though I verified that my network routes, VPC settings, and Security Groups were perfectly open, running `pnpm prisma db push` or `prisma db pull` kept throwing a generic network timeout error:

```bash
Error: P1001: Can't reach database server at re-rds...5432
```

But when I ran a native connection test using netcat (`nc`), the connection succeeded instantly in **0.03 seconds**! 

```bash
nc -zv re-rds...5432
# Ncat: Connected to 10.0.0.216:5432.
```

The network was wide open, but Prisma kept insisting it couldn't see the database.

---

## Why Did It Fail? (The Technical Breakdown)

1. **Prisma's CLI Engine is a Rust Binary:** When you run application code, it uses Node.js and standard drivers (like `pg`). But when you execute Prisma CLI commands (`prisma db push`), Prisma completely bypasses Node.js and fires up a pre-compiled **Rust binary engine** to talk to the database.
2. **The SSL Parameter Mismatch:** AWS RDS strictly requires encrypted connections using an SSL root certificate bundle (`rds-global-bundle.pem`). While my Node.js driver looked for the standard `sslrootcert` environment variable parameter, Prisma's Rust engine completely ignored it, expecting `sslcert` instead. Because it couldn't find or negotiate the certificate chain, the CLI binary's internal SSL handshake aborted, resulting in a generic `P1001` timeout.

---

## How I Fixed It

Instead of fighting a rigid, pre-compiled binary CLI engine, the cleanest solution was to bypass the Prisma CLI tool entirely on the server and handle schema execution **programmatically using Node.js**.

### Step 1: Write a Custom Programmatic Migration Runner

I created a custom `migrate.ts` script inside the `prisma/` directory. This script uses the app's existing `pg` connection pool—which already handles the AWS SSL certificate correctly—to read raw SQL migrations and apply them directly within a secure transaction block.

```typescript
// prisma/migrate.ts
import crypto from "crypto";
import fs from "fs/promises";
import path from "path";
import { fileURLToPath } from "url";
import dotenv from "dotenv";
import { createPgPool } from "../src/lib/pgPool.js";

dotenv.config();

// (The script reads /prisma/migrations, tracks state in a "_custom_migrations" table,
// and skips or applies migrations idempotently.)

type MigrationFile = {
  name: string;
  filePath: string;
  checksum: string;
  sql: string;
};

type DbClient = {
  query: (
    queryText: string,
    values?: unknown[],
  ) => Promise<{ rows: Array<{ name: string; checksum: string }> }>;
  release: () => void;
};

const __dirname = path.dirname(fileURLToPath(import.meta.url));
const migrationsDir = path.join(__dirname, "migrations");
const appliedMigrationsTable = '"_custom_migrations"';
const baselineExistingDb = process.env.BASELINE_EXISTING_DB === "true";

function checksum(contents: string): string {
  return crypto.createHash("sha256").update(contents).digest("hex");
}

async function loadMigrationFiles(): Promise<MigrationFile[]> {
  const entries = await fs.readdir(migrationsDir, { withFileTypes: true });

  const migrationDirectories = entries
    .filter((entry) => entry.isDirectory())
    .map((entry) => entry.name)
    .sort((left, right) => left.localeCompare(right));

  const migrations: MigrationFile[] = [];

  for (const directoryName of migrationDirectories) {
    const filePath = path.join(migrationsDir, directoryName, "migration.sql");
    const sql = await fs.readFile(filePath, "utf8");

    migrations.push({
      name: directoryName,
      filePath,
      checksum: checksum(sql),
      sql,
    });
  }

  return migrations;
}

async function ensureAppliedMigrationsTable(client: DbClient) {
  await client.query(`
    CREATE TABLE IF NOT EXISTS ${appliedMigrationsTable} (
      id SERIAL PRIMARY KEY,
      name TEXT NOT NULL UNIQUE,
      checksum TEXT NOT NULL,
      applied_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
  `);
}

async function getAppliedMigrations(client: DbClient) {
  const result = await client.query(
    `SELECT name, checksum FROM ${appliedMigrationsTable} ORDER BY id ASC;`,
  );

  return new Map(result.rows.map((row) => [row.name, row.checksum]));
}

async function applyMigration(client: DbClient, migration: MigrationFile) {
  await client.query("BEGIN");

  try {
    await client.query(migration.sql);
    await client.query(
      `INSERT INTO ${appliedMigrationsTable} (name, checksum)
       VALUES ($1, $2)
       ON CONFLICT (name) DO UPDATE SET checksum = EXCLUDED.checksum, applied_at = NOW();`,
      [migration.name, migration.checksum],
    );
    await client.query("COMMIT");
  } catch (error) {
    await client.query("ROLLBACK");
    throw error;
  }
}

async function markMigrationApplied(
  client: DbClient,
  migration: MigrationFile,
) {
  await client.query(
    `INSERT INTO ${appliedMigrationsTable} (name, checksum)
     VALUES ($1, $2)
     ON CONFLICT (name) DO UPDATE SET checksum = EXCLUDED.checksum, applied_at = NOW();`,
    [migration.name, migration.checksum],
  );
}

async function main() {
  const pool = createPgPool();
  const client = (await pool.connect()) as DbClient;

  try {
    await ensureAppliedMigrationsTable(client);

    const migrations = await loadMigrationFiles();
    const appliedMigrations = await getAppliedMigrations(client);
    const firstMigration = migrations[0];

    if (
      baselineExistingDb &&
      appliedMigrations.size === 0 &&
      firstMigration
    ) {
      console.log(`Baselining existing database with ${firstMigration.name}`);
      await markMigrationApplied(client, firstMigration);
      appliedMigrations.set(firstMigration.name, firstMigration.checksum);
    }

    for (const migration of migrations) {
      const appliedChecksum = appliedMigrations.get(migration.name);

      if (appliedChecksum) {
        if (appliedChecksum !== migration.checksum) {
          throw new Error(
            `Migration checksum mismatch for ${migration.name}. The database has a different version than ${migration.filePath}.`,
          );
        }

        console.log(`Skipping already applied migration ${migration.name}`);
        continue;
      }

      console.log(`Applying migration ${migration.name}`);
      await applyMigration(client, migration);
    }

    console.log("Database schema sync complete");
  } finally {
    client.release();
    await pool.end();
  }
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

### Step 2: Add Baselining for Existing Types

When migrating databases that have already been partially touched, PostgreSQL will throw conflicts if it tries to recreate an existing primitive (for example, `error: type "Highlight" already exists`). 

To prevent the script from crashing on deployment, we implemented **Database Baselining**. By feeding a `BASELINE_EXISTING_DB=true` flag, the script intelligently marks the initial migration state as recorded and shifts safely to subsequent updates without breaking.

```bash
BASELINE_EXISTING_DB=true pnpm run migrate:db
```

---

## Key Takeaways for Next Time

* **Don't mistake tool bugs for network bugs:** If `nc` or `telnet` hits a port instantly, your cloud network topology (VPC, Subnets, Security Groups) is healthy. The blockade is application-layer or tool-specific.
* **Decouple when tools are too rigid:** When a framework's native CLI binary gets in the way of environment-specific handshakes, writing a custom runner script that hooks directly into your verified application driver saves hours of environment headaches.
* **Baseline early:** Always write database migration steps defensively so they can run idempotently without choking on existing data structures or types.

> **Note:** Database baselining (`BASELINE_EXISTING_DB=true`) is a one-time operation during the initial deployment to an existing database. Once the first migration is marked as applied, you should remove the flag and let subsequent migrations run normally.