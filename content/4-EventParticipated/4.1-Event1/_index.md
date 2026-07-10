---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Event Report: FCAJ Community Day Workshop

## I. Event Information

**Event Name:** FCAJ Community Day Workshop
**Time:** 09:00 AM – 12:00 PM, Saturday, May 23, 2026
**Location:** Floor 26, Bitexco Financial Tower, 02 Hai Trieu Street, Ben Nghe Ward, District 1, Ho Chi Minh City
**Role:** Attendee
**Author:** Khoa – Final-year Software Engineering Student, HUTECH

---

## II. Event Overview

During my participation in the **First Cloud AI Journey** program, I had the opportunity to attend the **FCAJ Community Day Workshop**. This was a valuable technical event focusing on **AI in production**, **Amazon Q**, **CloudFront**, **system security**, **multi-agent architecture**, and real-world lessons from technology product development.

The event helped me broaden my understanding of how AI and cloud systems are deployed in enterprise environments. While university projects often end when the application works at a basic level, this workshop showed me that real-world systems require much more attention to security, cost optimization, scalability, reliability, input context quality, and risk control when using AI.

---

## III. Event Objectives

The main objectives of the event included:

* Sharing best practices for designing and deploying modern applications on AWS Cloud.
* Introducing safe and effective approaches to bringing AI into production.
* Presenting methods to optimize context when using Large Language Models.
* Introducing Amazon Q and AI Agent use cases in enterprise environments.
* Explaining how to improve performance and security with Amazon CloudFront.
* Sharing product development experience through a hackathon case study.
* Explaining the causes of non-deterministic behavior in Large Language Models.
* Presenting multi-agent architecture for corporate credit scoring.

---

## IV. Speakers

The speakers of the event included:

* **Ms. Vy Lam** – Senior Business Systems Analyst, VPBank.
* **Ms. Thao Nguyen** – GenAI Engineer, VIB.
* **Ms. Mai Nguyen** – GenAI Engineer, VIB.
* **Ms. Uyen Le** – GenAI Engineer, VIB.
* **Mr. Tinh Truong** – Platform Engineer, GoTymeX.
* **Mr. Thinh Nguyen** – DevOps Engineer, FCAJ.
* **Mr. Duc Dao** – Solutions Architect, Cloud Kinetics.
* **Mr. Pham Ng. Hai Anh** – Cloud Consultant, G-AsiaPacific Vietnam.

---

# V. Main Technical Contents

---

## 1. Context Is Everything – Bringing AI to Production

The session by **Mr. Tinh Truong** focused on an important topic when bringing AI into production: **Context Engineering**. Instead of only choosing a stronger model, the speaker emphasized that AI output quality depends heavily on how context is provided.

A strong context should include:

* **Goal:** the clear objective of the task.
* **Situation:** the real-world context of the system.
* **Constraints:** technical or business limitations.
* **Evidence:** relevant data, documents, or source code.

The speaker also pointed out common mistakes in enterprise AI usage, such as adding too many raw documents into prompts, repeating knowledge that the model already knows, or failing to provide specific technical constraints. These mistakes may cause the model to generate generic, inaccurate, or unusable outputs for production systems.

From this session, I learned that using AI effectively in real projects requires proper context design, relevant input selection, and clear requirements rather than short prompts that expect the model to understand everything automatically.

---

## 2. Friendly AI Assistant with Amazon Q

The session by **Mr. Pham Ng. Hai Anh** introduced **Amazon Q** and how AI Agents can support enterprise staff, especially non-technical users.

Amazon Q can connect to multiple data sources such as Amazon S3, internal databases, and enterprise tools. As a result, it can become an AI Assistant that helps users search information, summarize content, and automate daily tasks.

A practical example was the **PM Assistant**, which supports Project Managers by capturing meeting insights, generating Minutes of Meetings, drafting emails to stakeholders, and updating schedules for upcoming deliverables.

What impressed me is that Amazon Q is not just a question-answering chatbot. When configured with proper data, access controls, and guardrails, it can become a secure and useful enterprise assistant.

---

## 3. Maximizing Performance and Security with CloudFront

The session by **Mr. Thinh Nguyen** focused on using **Amazon CloudFront** to improve performance, reduce latency, and protect systems from risks such as traffic spikes or DDoS attacks.

The main topics included:

* Using CloudFront edge locations to bring content closer to users.
* Combining CloudFront with AWS WAF, AWS Shield, Route 53, and S3 for better security.
* Hiding the origin server to reduce exposure of the real system IP.
* Optimizing protocols such as HTTP/3 and QUIC/UDP.
* Using CloudFront Functions or Lambda@Edge to process logic at the edge.

From this session, I learned that CDN is not only used to speed up websites. In production systems, CloudFront is also an important protection layer that reduces origin load, mitigates edge-level attacks, and improves operational cost efficiency.

---

## 4. UTMorpho – From Concept to Product in 36 Hours

The VIB Engineering Team shared a case study about **UTMorpho** at LotusHacks 2026. This was a real example of developing a product within a short time frame of only 36 hours.

The idea of UTMorpho is to allow users to scan a hand-drawn UI sketch from paper or an iPad, then use AI to generate usable frontend source code. This is a practical solution for reducing repetitive work when transforming wireframes into user interfaces.

Some challenges included:

* AI-generated code being too long or unoptimized.
* API token limits during continuous testing.
* Time pressure during the hackathon.
* The need to reduce scope and focus on a working product instead of adding too many features.

The key lesson I learned is that when building a product under time pressure, scope control is more important than adding many features. A small but stable product is better than a large idea that cannot be completed.

---

## 5. Navigating Non-Determinism in Large Language Models

The session by **Mr. Duc Dao** explained why Large Language Models may not always produce exactly the same output, even when `temperature = 0`.

Some main causes include:

* Parallel floating-point computation on GPUs can create tiny numerical differences.
* Execution order inside GPU clusters may vary.
* API request batching can change the execution environment.
* Small logit differences may lead to different token selection.

The speaker also suggested mitigation strategies such as majority voting, strict schema enforcement, JSON mode, function calling, or regex constraints.

This session helped me understand that when building production AI applications, developers should not assume that models are perfectly deterministic. Instead, systems need output validation, constraints, and error-handling mechanisms.

---

## 6. Enterprise-Grade Multi-Agent Systems in Corporate Credit Scoring

The session by **Ms. Vy Lam** from VPBank was an impressive presentation about applying a **Multi-Agent AI System** to corporate credit scoring.

Instead of using a single AI Agent, the system is designed as a virtual credit committee with multiple specialized agents:

* Manager Agent.
* Financial Analyst Agent.
* Market Analyst Agent.
* Team Evaluator Agent.
* Risk Assessor Agent.

Each agent handles a specific perspective and can cross-check the results of others. This design improves transparency, reduces bias, and creates an audit trail for the decision-making process.

The speaker also emphasized important requirements when applying AI in finance, such as IAM, KMS, secret rotation, data governance, PII masking, network isolation, observability, human-in-the-loop, and compliance standards such as SOC 2, GDPR, and PCI DSS.

From this session, I learned that AI in enterprise systems cannot stop at a demo level. To move into production, the system needs strong security architecture, monitoring, data control, and validation workflows.

---

# VI. Key Takeaways

After attending the FCAJ Community Day Workshop, I gained several important takeaways:

* AI does not depend only on the model but also heavily on context quality.
* Context Engineering is a critical skill when building production AI applications.
* Amazon Q can strongly support enterprise workflow automation.
* CloudFront is not only a CDN but also an important layer for security and performance optimization.
* Product development requires scope control to avoid building too broadly without completion.
* LLMs are not perfectly deterministic, so output control mechanisms are necessary.
* Multi-Agent Architecture is suitable for complex problems requiring multiple expert perspectives.
* Security, governance, and observability are mandatory for bringing AI into real systems.

---

# VII. Applying the Knowledge

The knowledge from the workshop can be applied to my learning process and future projects in several ways:

* Apply Context Engineering when building AI chatbots.
* Design prompts with clear goals, context, constraints, and evidence.
* Use layered security architecture to protect user data.
* Learn more about Amazon Bedrock, Amazon Q, and AI Agent frameworks.
* Combine CloudFront, WAF, and S3 to protect and optimize websites.
* Design AI chatbot systems with output control to reduce hallucination.
* Study Multi-Agent Architecture for consulting or analysis-related use cases.
* Apply production-ready thinking instead of only building proof-of-concept demos.

---

# VIII. Event Photos / Evidence

![FCAJ Community Day Workshop](/my-hugo-site/images/Event/image.png)

# IX. Conclusion

The FCAJ Community Day Workshop was a very valuable event for me during my learning journey in cloud and AI. Through the sessions from experienced speakers, I gained a more practical understanding of how AI and cloud systems are built in enterprise environments.

The most important lesson I learned is that a production system should not only work functionally. It must also be secure, observable, cost-optimized, scalable, and risk-controlled. The knowledge about Context Engineering, Amazon Q, CloudFront, LLM non-determinism, and Multi-Agent Architecture will be an important foundation for my future real-world projects.