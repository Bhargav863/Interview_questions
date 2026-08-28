# DevOps Interview Preparation Guide
### AWS | Git | Jenkins — Basic to Scenario-Based Questions with Answers

---

## SECTION 1: AWS (18 Questions)

### Basic / Conceptual

**1. What is the difference between EC2 and Lambda?**

EC2 is an Infrastructure-as-a-Service offering — you provision a virtual machine, manage the OS, patching, scaling, and pay for the instance as long as it runs. Lambda is a serverless compute service — you upload code, AWS manages the underlying infrastructure, it scales automatically, and you pay only for execution time (per 100ms). Use EC2 for long-running, stateful, or custom-OS workloads; use Lambda for short-lived, event-driven, stateless tasks.

**2. Explain the difference between Security Groups and NACLs.**
- **Security Groups (SG)**: Operate at the instance/ENI level, are stateful (return traffic is automatically allowed), and only support "allow" rules.
- **NACLs (Network ACLs)**: Operate at the subnet level, are stateless (you must explicitly allow both inbound and outbound), and support both "allow" and "deny" rules, evaluated in rule-number order.

**3. What are the different types of Load Balancers in AWS?**
- **ALB (Application Load Balancer)** — Layer 7, supports path/host-based routing, ideal for HTTP/HTTPS microservices.
- **NLB (Network Load Balancer)** — Layer 4, handles millions of requests/sec with ultra-low latency, supports static IP/Elastic IP.
- **GLB (Gateway Load Balancer)** — Layer 3, used for deploying/scaling third-party virtual appliances (firewalls, IDS/IPS).
- **CLB (Classic Load Balancer)** — Legacy, rarely used now.

**4. What is the difference between S3 storage classes?**

Standard (frequent access), Standard-IA/One Zone-IA (infrequent access, cheaper storage but retrieval cost), Intelligent-Tiering (auto-moves objects based on access pattern), Glacier/Glacier Deep Archive (archival, cheapest, retrieval takes minutes to hours). Choice depends on access frequency and retrieval time tolerance.

**5. What is an IAM Role vs an IAM User?**

An IAM User represents a person or application with long-term credentials (access key/secret). An IAM Role is an identity with temporary permissions that can be assumed by users, applications, or AWS services (e.g., EC2 assuming a role to access S3) — no long-term credentials involved, which is more secure.

**6. What is the difference between EBS and EFS?**

EBS (Elastic Block Store) is block storage attached to a single EC2 instance at a time (within one AZ). EFS (Elastic File System) is a managed NFS file system that can be mounted concurrently by multiple EC2 instances across multiple AZs — good for shared storage use cases.

**7. What is a VPC and what are its core components?**

A VPC (Virtual Private Cloud) is an isolated virtual network within AWS. Core components: subnets (public/private), route tables, Internet Gateway (IGW) for public access, NAT Gateway (for private subnet outbound access), Security Groups, NACLs, and VPC peering/Transit Gateway for inter-VPC connectivity.

**8. What is Auto Scaling and how does it work?**

Auto Scaling automatically adjusts the number of EC2 instances in a group based on defined policies (target tracking, step scaling, scheduled scaling) tied to CloudWatch metrics like CPU utilization or request count. It ensures availability during load spikes and cost savings during low demand.

**9. What is the difference between RDS Multi-AZ and Read Replicas?**

Multi-AZ is for **high availability** — a synchronous standby copy in another AZ that fails over automatically during outages (not readable). Read Replicas are for **scaling read traffic** — asynchronous copies that can be in the same or different region and are readable, but don't provide automatic failover (in most engines).

**10. What is CloudFormation vs Terraform?**

CloudFormation is AWS-native Infrastructure-as-Code, using JSON/YAML templates, tightly integrated with AWS services and free to use. Terraform (by HashiCorp) is cloud-agnostic IaC using HCL, supports multi-cloud, and has a larger community/module ecosystem. Terraform is often preferred in multi-cloud shops; CloudFormation integrates faster with new AWS service features.

### Scenario-Based

**11. Your application on EC2 is experiencing intermittent 5xx errors and the CPU is spiking randomly. How do you troubleshoot?**
- Check CloudWatch metrics (CPUUtilization, memory if custom metric enabled, network).
- Check ALB target group health checks — are instances failing health checks and being cycled?
- Review application/system logs (via CloudWatch Logs agent).
- Check for Auto Scaling activity — is scale-out lagging behind traffic spikes?
- Look at connection limits (DB connections, ephemeral ports).
- Consider enabling detailed monitoring (1-min granularity) for faster diagnosis.
- If it's memory-related, check for memory leaks using tools like `top`/`htop` or APM tools.

**12. You need to migrate an on-prem MySQL database (500GB) to RDS with minimal downtime. How would you approach it?**

Use **AWS DMS (Database Migration Service)** with continuous replication (CDC — Change Data Capture): first do a full load of existing data, then DMS keeps syncing ongoing changes. Once the target RDS is caught up, do a brief cutover — stop writes to source, let DMS finish the last sync, then repoint the application to RDS. This minimizes downtime to just the cutover window instead of the full transfer.

**13. Your company wants to reduce EC2 costs by 40% without impacting production stability. What levers would you pull?**
- Right-size instances using Compute Optimizer / CloudWatch data.
- Move steady-state workloads to **Reserved Instances or Savings Plans**.
- Use **Spot Instances** for fault-tolerant/batch workloads.
- Implement Auto Scaling to avoid over-provisioning.
- Clean up unused EBS volumes, unattached Elastic IPs, idle load balancers.
- Move infrequently accessed S3 data to cheaper storage tiers/lifecycle policies.
- Consider Graviton (ARM) instances for compatible workloads (better price-performance).

**14. A junior engineer accidentally deleted an S3 bucket with critical data. What could have prevented this, and how do you recover?**

Prevention: Enable **S3 Versioning** (so deletes create delete markers, recoverable), enable **MFA Delete** on the bucket, and use **IAM policies/SCPs** to restrict `s3:DeleteBucket` to specific roles. Also maintain **cross-region replication** or backups via AWS Backup.
Recovery: If versioning was on, restore by removing delete markers/reverting to prior object versions. If the bucket itself was deleted without versioning, recovery is only possible from backups/replication — this highlights why backups are non-negotiable for critical data.

**15. Design a highly available, scalable 3-tier architecture on AWS for a web application.**
- **Web tier**: EC2 instances (or ECS/Fargate) behind an ALB, spread across multiple AZs, in an Auto Scaling Group, in public subnets (or private with ALB in public).
- **App tier**: EC2/ECS in private subnets, also load balanced internally, communicating with web tier.
- **DB tier**: RDS Multi-AZ (or Aurora) in private subnets, with Read Replicas for scaling reads.
- Use **NAT Gateway** for outbound internet access from private subnets, **S3 + CloudFront** for static assets, **Route 53** for DNS, and **CloudWatch + SNS** for monitoring/alerts.
- Security: SGs scoped tightly per tier, WAF on ALB/CloudFront, Secrets Manager for credentials.

**16. Your Lambda function is timing out intermittently when calling an RDS database. What's happening and how do you fix it?**
Likely a **connection pool exhaustion** issue — each Lambda invocation can open a new DB connection, and under high concurrency this exceeds RDS's max_connections limit, causing timeouts/hangs. Fix: use **RDS Proxy** to pool and share connections across Lambda invocations, reduce Lambda's reserved concurrency, or move to Aurora Serverless which handles this better. Also check Lambda's VPC configuration — ensure it's in the same VPC/subnet with a route to RDS and sufficient ENI capacity.

**17. How would you set up a CI/CD pipeline entirely using AWS-native services?**
CodeCommit (or GitHub via CodeStar connection) → **CodeBuild** (build & test) → **CodePipeline** (orchestrates stages) → **CodeDeploy** (deploys to EC2/ECS/Lambda) with deployment strategies like blue/green or canary. Add manual approval stages for production, and use CloudWatch/SNS for pipeline failure notifications.

**18. How do you secure secrets (DB passwords, API keys) used by applications running on AWS?**
Never hardcode secrets in code or environment variables in plaintext. Use **AWS Secrets Manager** (with automatic rotation) or **Systems Manager Parameter Store** (SecureString, using KMS encryption) for less frequently rotated secrets. Grant access via IAM roles scoped with least privilege, and audit access using CloudTrail.

---

## SECTION 2: GIT (16 Questions)

### Basic / Conceptual

**1. What is the difference between `git merge` and `git rebase`?**
`git merge` combines two branches by creating a new merge commit, preserving the full history of both branches (non-destructive). `git rebase` replays your branch's commits on top of another branch, creating a linear history — it rewrites commit history, which is cleaner but should be avoided on shared/public branches.

**2. What is the difference between `git fetch` and `git pull`?**
`git fetch` downloads commits/objects from the remote but does **not** merge them into your working branch — it just updates remote-tracking branches. `git pull` is essentially `git fetch` + `git merge` (or rebase, if configured) — it fetches and immediately integrates changes into your current branch.

**3. What is a detached HEAD state?**
It occurs when you check out a specific commit (not a branch), so HEAD points directly to a commit instead of a branch reference. Any new commits made here aren't attached to a branch, and can be lost after switching branches unless you create a new branch from that point (`git checkout -b new-branch`).

**4. Explain `git stash` and when you'd use it.**
`git stash` temporarily saves uncommitted changes (staged and unstaged) so you can switch context (e.g., to fix an urgent bug on another branch) without committing incomplete work. `git stash pop` reapplies and removes the stash; `git stash apply` reapplies but keeps it in the stash list.

**5. What is the difference between `git reset`, `git revert`, and `git checkout`?**
- `git reset` moves the branch pointer (and optionally working directory/index) to a different commit — rewrites history, risky on shared branches.
- `git revert` creates a **new commit** that undoes changes from a previous commit — safe for shared branches since it doesn't rewrite history.
- `git checkout` switches branches or restores files to a specific commit's state (in newer Git, `git switch`/`git restore` split this responsibility more clearly).

**6. What are the three states of a file in Git?**
Modified (changed but not staged), Staged (marked to be included in next commit via `git add`), and Committed (safely stored in the local repository's history).

**7. What is a `.gitignore` file used for?**
It specifies files/patterns Git should not track (e.g., build artifacts, `node_modules/`, `.env` secrets, IDE config files) — keeping the repository clean and preventing accidental commits of sensitive or unnecessary files.

**8. What's the difference between a Git tag and a branch?**
A branch is a movable pointer that advances as new commits are added. A tag is a fixed, immutable reference to a specific commit — typically used to mark release points (e.g., `v1.0.0`) and doesn't move.

**9. What is `git cherry-pick`?**
It applies a specific commit (identified by its hash) from one branch onto another, without merging the entire branch — useful for backporting a bug fix to a release branch without pulling in unrelated changes.

### Scenario-Based

**10. You accidentally committed sensitive data (an API key) and pushed it to the remote. How do you fix it?**
Simply deleting the file in a new commit isn't enough — the key still exists in history. You need to:
1. Rotate/revoke the exposed key immediately (most important step).
2. Use `git filter-repo` (or the older `BFG Repo-Cleaner`) to rewrite history and strip the sensitive data from all commits.
3. Force-push the cleaned history (`git push --force`) and have all collaborators re-clone or hard-reset their local copies, since history has changed.
4. Add the file pattern to `.gitignore` going forward, and consider pre-commit hooks/secret-scanning tools (like `git-secrets` or GitHub secret scanning) to prevent recurrence.

**11. Two team members made conflicting changes to the same file and both pushed. How do you resolve a merge conflict?**
When you `git pull` or `git merge` and Git flags a conflict, it marks the file with `<<<<<<<`, `=======`, `>>>>>>>` conflict markers. You manually edit the file to resolve the correct final content, remove the markers, then `git add <file>` and `git commit` (or `git rebase --continue` if rebasing) to complete the merge. In a team setting, it's best to communicate with the other author to understand intent before resolving, especially for logic-heavy conflicts.

**12. Your team uses Git Flow. A critical production bug needs an immediate fix, but the current release isn't done. What do you do?**
Create a **hotfix branch** directly off `main`/`master` (not off `develop`), fix the bug, test it, then merge the hotfix into **both** `main` (and tag a new patch release) **and** `develop` (so the fix isn't lost in the next release). This is the standard Git Flow hotfix pattern — it isolates the urgent fix from in-progress feature work.

**13. You need to undo the last 3 commits, but you've already pushed them to a shared remote branch. What's the safest approach?**
Since the branch is shared, avoid `git reset --hard` + force push (it rewrites history and can break others' work). Instead, use `git revert` to create new commits that undo the changes — this preserves history and is safe for collaborative branches. If a hard reset + force-push is unavoidable (e.g., agreed upon with the team), coordinate carefully and have everyone re-sync afterward.

**14. How would you find which commit introduced a bug in a large codebase?**
Use `git bisect` — it performs a binary search through commit history. You mark a known-good commit and a known-bad commit, and Git checks out commits in between; you test and mark each as good/bad until it narrows down to the exact commit that introduced the bug. This is far faster than manually checking commits one by one.

**15. Your feature branch has diverged significantly from `main` after weeks of development, causing painful merge conflicts. How do you handle this going forward?**
Rebase (or merge) `main` into your feature branch **regularly** (e.g., daily) instead of waiting until the end — this keeps divergence small and conflicts manageable. Alternatively, adopt trunk-based development with short-lived feature branches and frequent small PRs to avoid long-lived divergent branches altogether.

**16. In a CI/CD pipeline, how do you ensure only reviewed and tested code reaches the `main` branch?**
Use **branch protection rules**: require pull requests (no direct pushes to `main`), require at least one/two approving reviews, require status checks (CI build/tests) to pass before merging, and optionally require signed commits. This enforces code quality and traceability at the Git/repo-management level, complementing pipeline-level tests.

---

## SECTION 3: JENKINS (16 Questions)

### Basic / Conceptual

**1. What is Jenkins and why is it used?**
Jenkins is an open-source automation server used to implement CI/CD pipelines — automating building, testing, and deploying code whenever changes are pushed. It reduces manual effort, catches integration issues early, and standardizes the release process through plugins and pipeline-as-code.

**2. What is the difference between Freestyle jobs and Pipeline jobs?**
Freestyle jobs are configured via the Jenkins UI (point-and-click), simple but hard to version-control and not great for complex workflows. Pipeline jobs use a `Jenkinsfile` (Groovy-based, Declarative or Scripted syntax) stored in source control — enabling pipeline-as-code, versioning, code review, and complex multi-stage logic.

**3. What is the difference between Declarative and Scripted Pipelines?**
Declarative Pipeline uses a structured, opinionated syntax (`pipeline { agent {} stages {} }`) that's easier to read and validate, ideal for most use cases. Scripted Pipeline uses full Groovy syntax with more flexibility/control but is more complex and harder to maintain — used for advanced/custom logic that Declarative can't easily express.

**4. What are Jenkins agents/nodes, and why do we use them?**
Agents (formerly "slaves") are worker machines that execute the actual build/test/deploy steps, while the Jenkins **master/controller** handles scheduling, UI, and orchestration. Using distributed agents allows parallel builds, workload isolation, and matching builds to specific environments (OS, tools) without overloading the controller.

**5. What is a Jenkinsfile?**
A text file (checked into source control alongside the application code) that defines the entire CI/CD pipeline as code — stages, steps, environment variables, post-build actions — enabling versioning, code review, and reuse across environments.

**6. What are some commonly used Jenkins plugins?**
Git plugin (SCM integration), Pipeline plugin, Blue Ocean (UI), Credentials Binding, Docker Pipeline, Slack Notification, JUnit (test reporting), SonarQube Scanner, Kubernetes plugin (dynamic agents), and Artifactory/Nexus plugins for artifact management.

**7. How does Jenkins integrate with Git for triggering builds?**
Via **webhooks** — the Git server (GitHub/GitLab/Bitbucket) sends an HTTP POST to Jenkins on events like push/PR, triggering a build immediately. Alternatively, Jenkins can **poll SCM** at intervals (less efficient/real-time) using cron-like syntax in job config.

**8. What is the purpose of the `agent` directive in a Declarative Pipeline?**
It specifies where the pipeline (or a specific stage) executes — e.g., `agent any` (any available agent), `agent { label 'docker' }` (specific labeled agent), or `agent { docker 'image:tag' }` (run inside a container). It controls execution environment and resource allocation.

**9. How do you manage credentials securely in Jenkins?**
Use the built-in **Credentials Manager** (Jenkins → Manage Credentials) to store secrets (usernames/passwords, SSH keys, API tokens, secret text) encrypted at rest. Reference them in pipelines via `credentials()` binding or `withCredentials {}` blocks — never hardcode secrets in the Jenkinsfile or logs.

### Scenario-Based

**10. Your Jenkins pipeline works fine when run manually but fails intermittently when triggered by webhooks from multiple simultaneous PRs. What's likely wrong and how do you fix it?**
Likely a **concurrency/resource contention issue** — multiple builds running in parallel may be sharing the same workspace, agent, or external resource (e.g., same DB, same port). Fix by using `options { disableConcurrentBuilds() }` if builds must be sequential, or ensure each build uses an **isolated workspace** (`ws` directive) and unique resources (e.g., dynamic ports, containerized agents via Docker/Kubernetes plugin) so parallel builds don't collide.

**11. A pipeline stage that deploys to production should only run after manual approval. How do you implement this?**
Use the `input` step within the pipeline:
```groovy
stage('Approve Production Deploy') {
    steps {
        input message: 'Deploy to production?', ok: 'Deploy'
    }
}
```
This pauses the pipeline and waits for a designated user/group to approve via the Jenkins UI before proceeding to the deployment stage. You can also restrict who can approve using the `submitter` parameter.

**12. Your build artifacts need to be built once and then deployed to multiple environments (dev, staging, prod) without rebuilding. How do you structure this in Jenkins?**
Use **build promotion** pattern: build and archive the artifact once (`archiveArtifacts`), then use separate downstream pipeline stages/jobs that pull the **same artifact** (not rebuild) and deploy it to each environment sequentially with approvals gating progression to staging/prod. This ensures what's tested is exactly what's deployed (immutable artifact principle), often paired with a binary repository like Nexus/Artifactory to store and retrieve the versioned artifact.

**13. A Jenkins job is taking significantly longer to run than it used to, and the team suspects the Jenkins controller is overloaded. How do you troubleshoot and fix this?**
- Check controller CPU/memory (via Jenkins' built-in monitoring or `top`) — if it's maxed, the controller is likely running heavy builds itself instead of delegating to agents.
- Ensure **`agent any`/pipeline jobs aren't running directly on the controller** — configure the controller with 0 executors and offload all work to dedicated agent nodes.
- Set up more agents (static or dynamic via Kubernetes/Docker Cloud plugin) to distribute load and enable parallelism.
- Review plugin bloat/outdated plugins, which can degrade performance — audit and update/remove unused ones.
- Enable build log rotation/artifact cleanup to avoid disk I/O bottlenecks from excessive retained data.

**14. How would you design a Jenkins pipeline for a microservices architecture with 10+ independent services in separate repos?**
Use **Multibranch Pipeline** jobs per repository (auto-discovers branches/PRs and builds from each repo's own Jenkinsfile), combined with a **shared library** (common Groovy functions/pipeline templates for build/test/deploy logic) to avoid duplicating pipeline code across all 10+ services. For orchestrated releases spanning services, use a separate "orchestrator" pipeline that triggers/waits on individual service pipelines via `build job:` steps, or manage dependencies through a service mesh/deployment tool downstream instead of tightly coupling in Jenkins.

**15. Your pipeline needs to run different steps depending on which branch triggered the build (e.g., `feature/*` runs tests only, `main` builds and deploys). How do you implement this?**
Use conditional `when` blocks in Declarative Pipeline:
```groovy
stage('Deploy') {
    when {
        branch 'main'
    }
    steps {
        // deploy steps
    }
}
```
Combine with `expression {}` conditions for more complex branch-pattern logic if needed (e.g., matching `release/*` branches).

**16. After a Jenkins master crash, all job configurations and build history were lost because there was no backup. How do you prevent this going forward?**
Implement regular backups of the `JENKINS_HOME` directory (using the **ThinBackup** or **Backup** plugin, or a scheduled cron job/script) covering job configs, plugins, credentials, and build history — stored off-server (e.g., S3). Additionally, treat Jenkins configuration as code where possible: use **Configuration as Code (JCasC)** plugin to define Jenkins system config in YAML (version-controlled), and store all Jenkinsfiles in SCM (not Jenkins UI) so pipeline logic itself is never solely dependent on the Jenkins instance. For critical setups, consider running Jenkins on Kubernetes with persistent volume backups, or evaluate HA configurations.



