# Robot Shop Service Level Objectives

## Service
Robot Shop e-commerce demo deployed on Amazon EKS in us-east-1.

## Availability SLO
- Target: 99% successful availability checks over a rolling 30-day window.
- Measurement: successful HTTP health checks divided by all scheduled checks.
- Exclusions: planned maintenance must be documented separately.
- Status: target defined; 30-day compliance has not yet been measured.

## Latency SLO
- Target: 95% of successful storefront HTTP requests complete within 1 second.
- Measurement: request-duration histogram from application or ingress telemetry.
- Status: target defined; latency telemetry and baseline are pending.

## Error budget
For a 30-day period, a 99% availability target permits approximately
7 hours and 12 minutes of unavailability.

## Alerting and response
- Alert when availability falls below the SLO over a sustained evaluation window.
- Investigate pod readiness failures, failed HTTP checks, node pressure,
  and persistent-volume attachment or scheduling errors.
- Record incident start/end times, impact, mitigation, and follow-up actions.

## Current limitations
These are proposed objectives, not claims of measured compliance.
Prometheus/Grafana alerting, centralized logs, and distributed tracing must
be verified separately before they can be considered complete.
