Lab 10 – Cost & SLA Guardrail System for Serverless Workloads

Problem to solve
Build a guardrail system that monitors cost & performance of serverless components and alerts when thresholds are violated.

Core services

    EventBridge (scheduled rules + CloudWatch metric filters routed to bus)
    Lambda (guardrail evaluator)
    Aurora (guardrail_policies, violations tables)
    SNS (alerts to ops / students)
    SQS (buffer for high-volume metrics/alarms)

Criteria for success

    Aurora table defines guardrails such as:
        “Lambda X must not exceed Y invocations/day”
        “SQS queue Z must not exceed N messages visible”
        “Aurora cluster CPU must not stay > 70% for more than 10 minutes”
    EventBridge periodically triggers evaluator Lambda, which:
        queries CloudWatch metrics / Cost Explorer / etc.
        compares against rules in Aurora
        writes any violations into a violations table
        publishes alerts to SNS if serious.
    Demo: artificially break a guardrail (e.g., flood a test Lambda) and show:
        violation added to Aurora
        SNS alert received
    All policies + infra expressed in Terraform (students manage guardrails as code).
