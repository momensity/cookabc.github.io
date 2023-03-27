---
layout: post
title:  "DevOps, SRE, and Platform engineering"
date:   2023-03-21 09:52:39 +0800
categories: devops sre
---

### 1. [The difference between DevOps, SRE, and Platform engineering](https://twitter.com/alexxubyte/status/1637842698995642368)

> The concepts of DevOps, SRE, and Platform Engineering have emerged at different times and have been developed by various individuals and organizations.

![DevOps, SRE, and Platform engineering](https://pbs.twimg.com/media/FrrJQKCagAAllie?format=jpg&name=large)

**1. DevOps** as a concept was introduced in 2009 by Patrick Debois and Andrew Shafer at the Agile conference.

> They sought to bridge the gap between software development and operations by promoting a collaborative culture and shared responsibility for the entire software development lifecycle.

**2.SRE, or Site Reliability Engineering** was pioneered by Google in the early 2000s to address operational challenges in managing large-scale, complex systems.

> Google developed SRE practices and tools, such as the Borg cluster management system and the Monarch monitoring system, to improve the reliability and efficiency of their services.

**3. Platform Engineering** is a more recent concept, building on the foundation of SRE engineering.

> The precise origins of Platform Engineering are less clear, but it is generally understood to be an extension of the DevOps and SRE practices, with a focus on delivering a comprehensive platform for product development that supports the entire business perspective.

<br/>

---

<br/>

### 2. [Differences between CICD and GitOps](https://content.altexsoft.com/media/2020/05/word-image-53.png)

> The following diagram shows the difference between the GitOps and the CI/CD approach.

![CICD and GitOps](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Dluk3kqLgC18Y01LvjnPGw.png)

**GitOps is based on these key principles:**

- `Declarative`: all system components (infrastructure and applications) are described declarative
- `Versioned and Immutable`: the desired system state is stored in a version-controlled Git repository
- `Pulled Automatically`: approved changes are automated and applied to the system
- `Continues Reconciled/Synchronization`: Agents/Operators are used to ensure correctness and alert any divergence from desired state