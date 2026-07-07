# AWS Lambda – WAF, Bedrock, and Terraform Lab

## Welcome

If you're reading this, then Theo is probably recovering and a deer is leading tonight's lab.

First...

**Don't panic.**

You have already completed far more difficult labs than this one.

Everything in this repository is built from concepts that you have already learned.

- Lambda
- IAM
- CloudWatch
- API Gateway
- WAF
- DynamoDB
- EventBridge
- Terraform

Nothing here is "new magic."

This lab simply connects those services together.

---

# Remember

Cloud engineering is not about memorizing commands.

It is about understanding **how systems work together.**

If something doesn't work immediately...

**Stop.**

Think.

Read the error.

Look at CloudWatch.

Check IAM.

Verify Terraform.

Those are the same troubleshooting steps used by professional cloud engineers every day.

---

# Your Goal

By the end of this lab you should understand the following architecture.

```
                Internet
                    │
                    ▼
             API Gateway (REST)
                    │
                    ▼
                  AWS WAF
                    │
                    ▼
          CloudWatch WAF Logs
                    │
                    ▼
      waf_bedrock_analyzer Lambda
          │                 │
          │                 │
          ▼                 ▼
    DynamoDB           Amazon Bedrock
          │                 │
          └────────┬────────┘
                   ▼
        AI Security Analysis
```

This is no longer "just a Lambda."

This is a cloud-native security pipeline.

---

# Don't Rush

There is no prize for finishing first.

There **is** value in understanding why something works.

Take your time.

Read the Terraform.

Compare it to the ClickOps lab.

Notice how every AWS Console action becomes Infrastructure as Code.

---

# Troubleshooting Order

If something fails...

Do **not** randomly change code.

Instead ask these questions.

### Terraform

Did Terraform apply successfully?

```
terraform validate
terraform plan
terraform apply
```

---

### IAM

Does the Lambda execution role have the required permissions?

Especially:

- logs:FilterLogEvents
- dynamodb:PutItem
- bedrock:InvokeModel

---

### CloudWatch

Did the Lambda execute?

Look here first.

```
CloudWatch
→ Log Groups
→ /aws/lambda/waf-bedrock-analyzer
```

CloudWatch almost always tells you what happened.

---

### DynamoDB

Did the Lambda save the event?

Look for the table:

```
waf-events
```

Verify records are being inserted.

---

### Bedrock

Did Bedrock generate a response?

If not:

- Is the correct model selected?
- Is Bedrock model access enabled?
- Does the Lambda role have permission to invoke the model?

---

# Think Like an Engineer

Professional engineers do not immediately search Google.

They gather evidence.

They form a hypothesis.

They test it.

They verify the result.

That process is far more valuable than memorizing syntax.

---

# You Already Know More Than You Think

Think back over the last several months.

You've already built:

- Kubernetes clusters
- Terraform deployments
- CI/CD pipelines
- Cognito authentication
- RBAC
- WAF
- Lambda
- API Gateway
- Bedrock integrations

This lab simply combines those skills.

---

# Remember the Process

Observe

↓

Understand

↓

Build

↓

Test

↓

Verify

↓

Improve

That process is what makes an engineer.

---

# Finally...

Have fun.

You're building something that resembles a real cloud security architecture.

Every AWS service in this repository exists because organizations use them in production every day.

Take your time.

Ask questions.

Experiment.

Break things.

Fix them.

That's how engineers learn.

Good luck.

— Broken Theo from bed.

## A Message from Theo

As many of you know, I'm currently recovering from a hit-and-run accident and have been ordered to stay in bed for a week. My class admins and Chewbacca have both made it very clear that if I try to teach tonight, they'll probably stage another intervention.

So... you're stuck with some Deer from Boston for this lab.

Please give them the same respect and attention that you would give me. They're helping keep the class moving forward while I recover.

Most importantly...

**Think.**

Don't immediately ask AI.

Don't immediately ask Discord.

Don't immediately ask the instructor.

Read the error.

Read CloudWatch.

Read the Terraform.

Read the IAM policy.

The answer is usually there.

This lab is designed to teach you how cloud systems work together.

You've got this.

Now go build something awesome.

— Broken Theo (from bed 🛏️)
