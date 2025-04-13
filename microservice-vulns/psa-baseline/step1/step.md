# Step 1: Enforce Pod Security Standards

## Objective
Create a Pod Security Admission (PSA) rule in the 'audited' namespace with baseline enforcement level. Then attempt to create a Pod that violates the baseline restrictions.

## Configuration
1. Create the 'audited' namespace
2. Apply baseline Pod Security Standard (PSS)
3. Create a Pod that violates baseline restrictions

## Solution

<details>
<summary>Click here to see the solution</summary>

1. Create the namespace:
```bash
kubectl create namespace audited
```{{EXEC}}

2. Label the namespace with baseline enforcement:
```bash
kubectl label namespace audited pod-security.kubernetes.io/enforce=baseline
```{{EXEC}}

3. Create a Pod that violates baseline restrictions (running as root):
```bash
tee psa-violation.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: psa-violation
  namespace: audited
spec:
  containers:
  - name: test
    image: nginx
    securityContext:
      privileged: true
EOF
```{{EXEC}}

4. Attempt to create the Pod:
```bash
kubectl apply -f psa-violation.yaml
```{{EXEC}}

5. Observe the error message indicating the Pod creation was blocked
</details>

## Verify
Run the verification script to confirm the scenario was completed successfully:
```bash
./verify.sh
```{{EXEC}}
