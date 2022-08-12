---
title: "K8S security"
date: 2022-08-12T13:33:37Z
---

Host Security
=============
* Keep your K8S daemons / distro up to date.
  - https://www.flatcar.org/
* Ensure RBAC is utilized in a proper manner.
  - `ClusterRole`
  - `ClusterRoleBinding`
  - `Role`
  - `RoleBinding`
  - https://github.com/alcideio/rbac-tool
* Ensure serviceaccount secrets don't get mounted automatically.
  - `pod.spec.automountServiceAccountToken`
* Run as non-root.
  - `podsecuritypolicies.spec.runAsUser`
  - `podsecuritypolicies.spec.runAsGroup`
* Mount root filesystems as read-only.
  - `podsecuritypolicies.spec.readOnlyRootFilesystem`
* Disallow privilege escalations.
  - `podsecuritypolicies.spec.allowPrivilegeEscalation`
* Make use of seccomp.
  - `pod.spec.securityContext.seccompProfile`
* Enforce SELinux.
  - `podsecuritypolicies.spec.seLinux`
  - `pod.spec.securityContext.seLinuxOptions`
* Make use of sandboxed pods:
  - https://gvisor.dev/
  - https://katacontainers.io/

Network Security
================
* Limit access to the API server / cluster using a firewall.
* Apply network policies.
  - `NetworkPolicy`
  - https://cilium.io/
  - https://www.tigera.io/project-calico/
* Ensure traffic is E2E encrypted (e.g. using a service mesh).
  - https://istio.io/
* Automate PKI management.
  - https://cert-manager.io/

Application security
====================
* Only allow signed images.
* https://resources.infosecinstitute.com/topic/building-container-images-using-dockerfile-best-practices/
* https://github.com/aquasecurity/trivy
* https://github.com/quay/clair
* https://snyk.io/product/open-source-security-management/
* https://snyk.io/product/container-vulnerability-management/

Threat detection
================
* https://falco.org/
