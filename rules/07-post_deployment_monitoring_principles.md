# Post-Deployment Monitoring: Core Principles & Guidelines

Effective post-deployment monitoring is crucial for system reliability, performance optimization, and issue resolution. These principles guide all agents involved in defining, implementing, and utilizing monitoring capabilities (primarily `devops-master`, `integration-orchestrator`, `performance-optimizer`, and overseen by `sparc-orchestrator`).

## 1. Overall Monitoring Philosophy

*   **Proactive Observation**: Monitoring is not just for when things go wrong; it's for understanding system behavior under normal and peak loads to anticipate issues.
*   **Data-Driven Decisions**: Monitoring data (logs, metrics, traces) should inform decisions about scaling, optimization, and feature enhancements.
*   **Actionable Insights**: Alerts and dashboards should provide clear, actionable information, not just raw data.
*   **Holistic View**: Monitor all layers of the stack: infrastructure, application, business processes.
*   **Continuous Improvement**: Monitoring strategies and configurations should evolve with the system.

## 2. Key Phases & Responsibilities in Monitoring Setup

While specific agents lead different aspects, the overall workflow involves:

| Phase                 | Primary Actions                                                                                                | Key Agents Involved                                  | Tool Preferences (General)                                 |
|-----------------------|----------------------------------------------------------------------------------------------------------------|------------------------------------------------------|------------------------------------------------------------|
| **1. Definition & Planning** | Define SLIs/SLOs, identify key metrics (performance, business, health), plan log structure, design alert conditions. | `integration-orchestrator`, `performance-optimizer`, `architect-enhanced` | Documentation (`insert_content`, `write_to_file`)            |
| **2. Implementation & Setup** | Configure monitoring tools, instrument code for metrics/tracing, set up log aggregation, deploy dashboards & alerts. | `devops-master`, `code-architect`, `frontend-architect` | IaC tools (`apply_diff`), code changes (`apply_diff`)        |
| **3. Baseline Collection** | Collect initial metrics under normal load to establish performance baselines.                                 | `devops-master`, `performance-optimizer`             | Monitoring tools (`execute_command`)                         |
| **4. Ongoing Analysis**   | Examine logs, metrics, and alerts to identify patterns, anomalies, and potential issues.                       | `debugging-specialist`, `performance-optimizer`, `devops-master` | Log analysis tools, `read_file`                            |
| **5. Diagnosis & Root Cause** | Pinpoint root causes of performance degradation or errors.                                                   | `debugging-specialist`, `performance-optimizer`      | Diagnostic scripts (`apply_diff`), `execute_command`         |
| **6. Remediation**        | Implement fixes or optimizations based on findings.                                                            | `code-architect`, `frontend-architect`, `devops-master` | Code/config changes (`apply_diff`)                         |
| **7. Verification**       | Confirm improvements, ensure no regressions, and establish new baselines.                                        | `quality-assurance`, `performance-optimizer`         | Testing tools, monitoring tools (`execute_command`)        |

## 3. Non-Negotiable Monitoring Requirements (System-Wide)

*   **Baseline Metrics**: Establish and record baseline performance metrics **BEFORE** major changes or post-deployment.
*   **Contextual Logs**: Logs **MUST** include proper context: precise timestamps (UTC), severity levels, correlation IDs (for distributed tracing), service/component names, and relevant request/user identifiers.
*   **Error Reporting**: Implement comprehensive error handling and reporting across all services. Errors should be logged with sufficient detail for diagnosis.
*   **Critical Alerts**: Set up alerts for critical thresholds related to SLOs, error rates, resource saturation, and security events.
*   **Documented Configuration**: All monitoring configurations (tool settings, alert rules, dashboard queries, metric definitions) **MUST** be documented and version-controlled (ideally as code). `documentation-master` and `devops-master` collaborate.
*   **Minimal Performance Impact**: Monitoring instrumentation and tools **MUST** be chosen and configured to have minimal overhead on system performance.
*   **Data Protection in Logs**: **NEVER** log sensitive data (PII, credentials, raw tokens, financial details) unless explicitly required for security auditing and properly anonymized or masked. `security-guardian` provides guidance.
*   **Audit Trails**: Maintain audit trails for significant system events and configuration changes.
*   **Log Management**: Implement proper log rotation, retention policies, and archiving strategies, managed by `devops-master`.
*   **Comprehensive Coverage**: Strive for monitoring coverage across all critical system components, services, and user journeys.

## 4. Monitoring Best Practices (System-Wide)

*   **USE Method**: Apply the "Utilization, Saturation, Errors" method for resource monitoring (CPU, memory, disk, network).
*   **RED Method**: Implement the "Rate, Errors, Duration" method for service monitoring (API endpoints, microservices).
*   **SLIs & SLOs**: Clearly define Service Level Indicators (SLIs) and Service Level Objectives (SLOs) for key services and user journeys. These drive alerting.
*   **Structured Logging**: Use consistent, machine-parseable log formats (e.g., JSON) across all services to facilitate aggregation and analysis.
*   **Distributed Tracing**: Implement distributed tracing in microservice architectures to track requests across service boundaries.
*   **Key Performance Dashboards**: `devops-master` and `integration-orchestrator` collaborate to set up dashboards displaying KPIs, system health, and SLO adherence.
*   **Runbooks**: Develop runbooks (operational guides) for common alerts and failure scenarios, maintained by `documentation-master` and `devops-master`.
*   **Automated Tasks**: Automate routine monitoring tasks (e.g., log rotation, baseline reporting) where possible.
*   **Anomaly Detection**: Explore anomaly detection capabilities of monitoring tools for identifying unusual patterns.
*   **Alerting Strategy**: Establish proper alerting thresholds to minimize alert fatigue while ensuring timely notification for actionable issues. Implement escalation policies.
*   **Historical Data**: Maintain historical metrics for trend analysis, capacity planning, and long-term performance review.

## 5. Log Analysis Guidelines (Applicable to `debugging-specialist`, `devops-master`)

| Log Type           | Key Information to Extract                                 | Analysis Approach                                     |
|--------------------|------------------------------------------------------------|-------------------------------------------------------|
| Application Logs   | Error messages, stack traces, request IDs, timing, business logic flow | Pattern recognition, error clustering, anomaly detection |
| System/Infra Logs  | CPU/memory/disk/network utilization, hardware errors, kernel messages | Resource bottleneck identification, correlation with app issues |
| Security Logs      | Auth attempts (success/failure), access patterns, policy violations, IDS/IPS alerts | Anomaly detection, threat hunting, incident forensics   |
| Database Logs      | Slow queries, errors, lock contention, connection issues, replication status | Query optimization, schema analysis, indexing review    |
| Network Logs/Flows | Latency, packet loss, connection rates, firewall denies, traffic sources/destinations | Topology analysis, bottleneck identification, security review |

*   **Centralized Logging**: `devops-master` ensures logs from all components are aggregated into a central system (e.g., ELK stack, CloudWatch Logs).
*   **Parsing & Indexing**: Implement efficient log parsing and indexing for fast searching and analysis.
*   **Search & Filtering**: Utilize powerful search and filtering capabilities of the logging platform.
*   **Log-Based Alerting**: Configure alerts based on specific log patterns or error frequencies.

## 6. Performance Metrics Framework (Guidance for `performance-optimizer`, `integration-orchestrator`)

### System-Level Metrics (Monitored by `devops-master` tools)
*   CPU utilization (overall, per-core, per-process)
*   Memory usage (total, free, active, inactive, swap)
*   Disk I/O (throughput, IOPS, latency, queue length, disk space)
*   Network I/O (bandwidth, packets in/out, errors, retransmits, latency)
*   System load average

### Application-Level Metrics (Instrumented by `code-architect`/`frontend-architect`, defined by `integration-orchestrator`)
*   Request Rate (per endpoint/service)
*   Error Rate (HTTP 5xx, 4xx, custom application errors)
*   Response Time/Latency (average, median, p90, p95, p99)
*   Throughput (e.g., transactions per second, messages processed per minute)
*   Resource utilization within the application (e.g., thread pool usage, queue depths, cache hit/miss rates)
*   Dependencies health (status of databases, external APIs)

### Database Metrics (Monitored by `devops-master`, analyzed by `performance-optimizer`)
*   Query execution time & frequency
*   Connection pool statistics
*   Index hit/miss rates, scan efficiency
*   Cache hit/miss ratios
*   Replication lag, transaction rates, deadlocks

### Custom Business Metrics (Defined with Product Owner, implemented by relevant coding agents)
*   User engagement (e.g., active users, session duration)
*   Conversion rates (e.g., sign-ups, purchases)
*   Feature adoption rates
*   Business transaction completion success/failure rates

## 7. Alerting System Design Principles (Implemented by `devops-master` based on definitions from other agents)

### Alert Severity Levels
1.  **P1/Critical**: System down, significant user impact, data loss/corruption. Requires IMMEDIATE automated or human intervention.
2.  **P2/Warning**: Potential service degradation, approaching critical thresholds, risk of SLO breach. Requires prompt attention.
3.  **P3/Info**: Noteworthy events, non-critical issues, early warnings for trends. For awareness, may not require immediate action.

### Alert Configuration Guidelines
*   **Thresholds Based on Baselines & SLOs**: Alerts should trigger when metrics deviate significantly from established baselines or threaten SLOs.
*   **Progressive Alerting**: Design alerts to warn before a situation becomes critical.
*   **Rate-of-Change Alerts**: Useful for detecting sudden spikes or drops.
*   **Reduce Noise**: Aggregate related alerts, use alert flapping detection, and tune thresholds to avoid alert fatigue.
*   **Clear Escalation Paths**: Define who gets notified for which alerts and how escalations occur.
*   **Actionable Alerts**: Alerts MUST provide context and ideally suggest initial troubleshooting steps or link to relevant runbooks.
*   **Maintenance Windows**: Implement alert suppression during planned maintenance.
*   **Alert Correlation**: Utilize monitoring tool features to correlate related alerts and identify root causes more quickly.

## 8. Tool Preferences (General - Specific commands depend on chosen monitoring stack)

*   **Configuration & Dashboards (`devops-master`, `integration-orchestrator`)**:
    *   IaC tools (Terraform, Ansible, etc.) using `<apply_diff>` for defining monitoring resources.
    *   Direct configuration file changes using `<apply_diff>` (e.g., Prometheus `alerts.yml`, Grafana dashboard JSON).
    *   `<insert_content>` for adding new alert rules or dashboard panels to existing files.
*   **Collecting Metrics/Status (`devops-master`, `performance-optimizer`)**:
    *   `<execute_command>` to query monitoring system APIs, run diagnostic CLI tools (e.g., `kubectl top pods`, `docker stats`, `vmstat`).
*   **Log Analysis (`debugging-specialist`, `devops-master`)**:
    *   `<read_file>` for direct log file inspection (small scale or specific files).
    *   `<execute_command>` to run log query commands if a CLI to the logging platform is available.
    *   For large-scale analysis, agents would typically describe the query needed for a human or a specialized log query MCP if available.
*   **Code Instrumentation (`code-architect`, `frontend-architect`)**:
    *   `<apply_diff>` to add metric collection libraries (e.g., Prometheus client, OpenTelemetry SDK) and custom metric reporting code.

This document provides a foundational set of principles. Specific monitoring tools and techniques will be detailed in mode-specific rules for agents like `devops-master` or in project-specific documentation.