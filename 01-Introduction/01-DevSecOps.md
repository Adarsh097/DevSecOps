**DevSecOps** stands for **Development + Security + Operations**.

It is a practice where **security is integrated into every stage of the software development lifecycle (SDLC)** instead of being handled at the end.

---

## Simple Definition

DevSecOps means building software quickly (development), running it reliably (operations), and keeping it secure (security) at the same time.

---

## Traditional vs DevSecOps

### Traditional Approach

* Development → Operations → Security
* Security is handled at the end
* Vulnerabilities are found late

### DevSecOps Approach

* Development, Security, and Operations work together
* Security is introduced early (“shift left”)
* Issues are detected and fixed sooner

---

## Key Principles of DevSecOps

1. **Shift Left Security**
   Security starts from the coding phase. Developers follow secure coding practices.

2. **Automation**
   Security checks are automated in CI/CD pipelines to save time and ensure consistency.

3. **Continuous Monitoring**
   Applications are monitored in production to detect threats and vulnerabilities.

4. **Shared Responsibility**
   Security is not just the responsibility of a security team; everyone is involved.

---

## DevSecOps Lifecycle

1. Planning
   Define security requirements

2. Development
   Write secure code and perform code reviews

3. Build
   Scan dependencies for vulnerabilities

4. Testing
   Perform security testing (SAST, DAST)

5. Deployment
   Use secure configurations for infrastructure

6. Monitoring
   Continuously monitor and respond to threats

---

## Common DevSecOps Tools

* Code security: SonarQube, Checkmarx
* Dependency scanning: Snyk, Dependabot
* Container security: Trivy, Aqua Security
* CI/CD: Jenkins, GitHub Actions
* Cloud security: AWS Security Hub

---

## Example

In a Spring Boot application:

* During development, code is scanned for vulnerabilities
* During build, dependencies are checked
* During deployment, Docker images are scanned
* In production, monitoring tools detect suspicious behavior

This ensures the application remains secure throughout its lifecycle.

---

## Why DevSecOps is Important

* Faster software delivery
* Improved security
* Lower cost of fixing issues
* Reduced risk of security breaches

---

## Interview One-Liner

DevSecOps is the practice of integrating security into every stage of the DevOps pipeline using automation and shared responsibility.

- Git -> RBAC, PreCommit Hook
- IAC -> Terraform -> Vault
- Scripting -> python -> Flask -> 2.3.4,2.3.5 -> no vunlerability package use
- Containers -> not root user, distroless image, multistage builds
- K8s ->  vpc -> private subnet -> eks
- CI/CD -> stages 