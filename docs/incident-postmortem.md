# Incident Postmortem: Redis Scheduling Failure

## Summary
During the Robot Shop EKS deployment, Redis remained Pending and the
storefront stack was initially incomplete.

## Impact
Redis was unavailable while its pod could not be scheduled. This delayed
validation of the complete application.

## Detection
Kubernetes pod status and events showed FailedScheduling errors, including
insufficient memory, pod-capacity constraints, and persistent-volume node
affinity requirements.

## Root cause
The two original t3.small worker nodes had limited allocatable memory and
pod capacity. Redis used a bound EBS volume in us-east-1f, restricting
eligible placement to a worker in that Availability Zone.

## Resolution
1. Enabled the EBS CSI driver and its required Pod Identity integration.
2. Increased the managed node group's desired size from two to three workers.
3. Temporarily scaled the EBS CSI controller deployment to one replica
   to free reserved memory on the Redis-eligible node.
4. Redis subsequently reached Running and Ready.

## Data protection
The existing Redis PVC and PV were preserved. The volume was not deleted.

## Verification
Kubernetes verification showed the Robot Shop application pods Running and Ready.
The Helm release status and external HTTP response still require final
verification after the cluster API connectivity issue.

## Lessons learned
- Include CSI drivers and their IAM permissions in cluster bootstrap.
- Size worker nodes for system pods as well as application workloads.
- Check persistent-volume Availability Zone affinity before scheduling stateful pods.
- Monitor pending pods, memory requests, and pod limits.
- Avoid scaling down critical infrastructure components without checking
  availability and resource implications.

## Follow-up actions
- Verify Helm reports STATUS: deployed.
- Test the storefront externally.
- Add resource and pod scheduling alerts.
- Complete and validate centralized logging, tracing, and dashboard configuration.
- Review node sizing and restore the CSI controller's appropriate replica count
  after confirming sufficient capacity.
