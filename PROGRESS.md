# Checkpoint 24 — Task 1 Progress (remove this file before submission)

## Status

| Step | Status |
|---|---|
| Dockerfile — ROS Noetic + TortoiseBot + waypoints | Done |
| Docker image pushed to DockerHub | Done |
| Jenkins 2.504.3 installed + plugins configured | Done |
| GitHub App created + credential added to Jenkins | Done |
| smee.io webhook relay configured | Done |
| Multibranch Pipeline job created in Jenkins | Done |
| Jenkinsfile written | Done |
| Push Jenkinsfile to GitHub + test PR trigger | **Next** |

---

## Each Session — Start Jenkins

```bash
kill $(cat ~/webpage_ws/jenkins/jenkins.pid)
cd ~/simulation_ws/src/ros1_ci
bash jenkins-infra/scripts/jenkins_bootstrap.sh
jenkins_address    # get browser URL for this session — changes every session
```

---

## First Session Only — Full Setup

```bash
# 1. Start Jenkins
cd ~/simulation_ws/src/ros1_ci
bash jenkins-infra/scripts/jenkins_bootstrap.sh

# 2. Install plugins (once — auto-skipped on future runs)
bash jenkins-infra/scripts/install_plugins.sh

# 3. Restart to activate plugins
kill $(cat ~/webpage_ws/jenkins/jenkins.pid)
bash jenkins-infra/scripts/jenkins_bootstrap.sh

# 4. Open browser
jenkins_address
cat ~/webpage_ws/jenkins/secrets/initialAdminPassword
```

Complete the Jenkins setup wizard, then:

**Add GitHub App Credential:**
Manage Jenkins → Credentials → System → Global credentials (unrestricted) → Add Credentials
- Kind: `GitHub App`
- App ID: (from GitHub App page)
- Private Key: paste converted `.pem` (convert: `openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in original.pem -out converted.pem`)

**Create Multibranch Pipeline job** named `ros1-ci`:
- Branch Sources → GitHub → credential → Owner → Repository: `ros1_ci`
- Add behavior: Discover pull requests from origin → Merging with target branch
- Add behavior: Status Check Properties → Skip GitHub Branch Source notifications
- Build config: by Jenkinsfile → Script Path: `Jenkinsfile`
- Scan Triggers: Periodically if not otherwise run → 1 minute

---

## smee.io (one-time, already configured)

smee URL: `https://smee.io/WOi7gUgFDGQcVAQ`
Set permanently in `~/.bashrc` on Construct: `export SMEE_URL="https://smee.io/WOi7gUgFDGQcVAQ"`
GitHub App webhook URL already set to smee URL — no changes needed each session.
Bootstrap auto-starts smee when `SMEE_URL` is in environment.