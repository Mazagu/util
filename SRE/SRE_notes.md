# SRE Overview
### **What is Site Reliability Engineering (SRE)?**

Site Reliability Engineering (SRE) is an engineering discipline that applies software development principles to operations tasks. It aims to build scalable, reliable systems by automating manual operations, managing risks, and optimizing the balance between innovation and system stability.

Key areas of SRE include:  
- **Reliability:** Ensuring high availability and performance.  
- **Monitoring:** Implementing tools and metrics to observe system health.  
- **Incident Response:** Managing and resolving outages or degradations efficiently.  
- **Automation:** Reducing toil through scripts, tools, and infrastructure-as-code.  
- **Capacity Planning:** Ensuring systems can handle traffic and growth without failure.

### **Who Does SRE?**

SRE roles are typically filled by engineers who have a mix of skills in software development, systems administration, and operations. This includes:  
- **Dedicated SRE Teams:** Teams entirely focused on SRE practices, usually in larger organizations.  
- **Hybrid Roles:** Developers or operators with some SRE responsibilities in smaller or DevOps-oriented environments.  
- **Cross-functional Product Teams:** In modern organizations, SRE principles are often shared across product development teams.


### **Expectations in Modern Software Engineering Roles**

Today, many software engineers are expected to contribute to reliability alongside their primary development tasks. This integration of responsibilities is sometimes referred to as **"You build it, you run it."**

**Typical expectations include:**  
1. **Participation in On-Call Rotations:** Engineers may join on-call schedules to support the systems they build.  
2. **Incident Management:** Responding to and learning from outages to improve system reliability.  
3. **Monitoring & Alerting:** Implementing metrics and dashboards for observability.  
4. **Infrastructure Knowledge:** Using tools like Kubernetes, Terraform, or CI/CD pipelines to deploy and manage applications.  
5. **Collaboration with SRE Teams:** Partnering with dedicated SREs to build robust, scalable systems.  

### **Balancing Product Development with SRE Responsibilities**  

- **Small Teams:** Engineers often have to balance feature development with operational tasks.  
- **Large Teams:** Responsibilities are more delineated, but developers may still focus on building resilient code and designing for operability.  
- **Cultural Shift:** Modern engineering culture expects developers to consider operational implications (e.g., SLAs, SLIs, SLOs) from the start.

By blending product development and SRE principles, organizations aim to achieve **faster development cycles** without compromising **system reliability**.

### **Comparison of Approaches to Manage Operational Concerns in Software Engineering**

Managing operational concerns in software engineering varies significantly depending on the team's size, maturity, and culture. Below is a comparison of the most common approaches:

| **Approach**                 | **Description**                                                                                 | **Advantages**                                                   | **Disadvantages**                                              | **Best For**                                      |
|------------------------------|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------|---------------------------------------------------------------|---------------------------------------------------|
| **Traditional Operations Team** | A separate operations team handles all deployment, monitoring, and incident management tasks. | - Clear division of responsibilities<br>- Operations expertise   | - Siloed communication<br>- Slower deployments<br>- Less developer accountability | Large organizations with legacy systems          |
| **DevOps**                   | Developers and operators collaborate closely, sharing responsibilities for deployment and reliability. | - Improved collaboration<br>- Faster feedback loops<br>- Continuous delivery focus | - Requires cultural change<br>- Can lack clarity in responsibilities | Organizations transitioning to modern workflows  |
| **SRE (Dedicated Team)**     | A specialized team applies SRE principles to manage system reliability and automate operations. | - Strong reliability focus<br>- Expertise in automation<br>- Reduces toil | - Can create a new silo<br>- Requires investment in SRE roles and tools | Companies with complex, high-scale systems       |
| **Integrated SRE/Dev Teams** | Product teams adopt SRE practices and handle their system's operational concerns.             | - Increased accountability<br>- Faster issue resolution<br>- Seamless feedback | - Higher cognitive load on developers<br>- Requires training and mindset shift | Small-to-medium teams with modern infrastructure |
| **"You Build It, You Run It"** | Developers own end-to-end responsibility for their services, including operations and on-call duties. | - Maximum ownership<br>- Clear accountability<br>- Faster iteration | - Burnout risk<br>- Requires robust tooling and culture<br>- Not scalable for complex systems | Startups or small teams                          |

---

#### **Key Factors to Consider**

1. **Team Size and Structure:** Smaller teams benefit from integrated approaches (e.g., DevOps, "You Build It, You Run It"), while larger organizations may require dedicated operations or SRE teams.  
2. **System Complexity:** Highly complex systems often require dedicated SRE teams to handle specialized concerns.  
3. **Cultural Readiness:** Success in DevOps or SRE models depends on cultural alignment and willingness to adapt.  
4. **Operational Maturity:** Teams with strong processes and automation can handle more integrated approaches.  
5. **Business Needs:** Fast-paced environments prioritize speed, while mission-critical systems prioritize reliability.

#### **Recommendations by Context**

| **Context**                       | **Recommended Approach**                                |
|-----------------------------------|--------------------------------------------------------|
| Startup with small, nimble teams  | "You Build It, You Run It" or DevOps                   |
| Mid-sized company scaling up      | DevOps with elements of SRE                            |
| Enterprise with legacy systems    | Traditional Ops transitioning to DevOps or SRE         |
| Large-scale, high-availability system | Dedicated SRE team with close collaboration to Dev teams |

Selecting the right model involves balancing operational needs with team capabilities, ensuring long-term sustainability and business alignment.

# Key Concepts in SRE
## Product: Change Velocity (features) and Service's SLO (stability)

1. **Structural Conflict**:
   - Product development teams and SRE teams often have conflicting goals: development wants rapid innovation, while SRE seeks system stability.
   - The key conflict lies between **pace of innovation** and **product stability**.

2. **Error Budget**:
   - The introduction of an **error budget** resolves this conflict.
   - **100% reliability** is not practical. In fact, aiming for 100% availability is counterproductive as users can’t distinguish between 100% and 99.999% uptime.
   - Other systems (like ISPs, laptops, Wi-Fi) contribute to availability, so striving for perfection in one service offers minimal benefit.

3. **Defining the Right Reliability Target**:
   - The **reliability target** is a business or product decision, not a technical one. Factors to consider:
     - How much downtime will users tolerate?
     - What alternatives do users have if the service is down?
     - How does availability affect user behavior and product usage?
   
   - The **availability target** defines the error budget:
     - E.g., 99.99% availability leaves a **0.01% error budget** (downtime allowed).

4. **Spending the Error Budget**:
   - The error budget can be spent in any way that doesn’t exceed the allocated downtime.
   - The development team aims to innovate and roll out features quickly, which can use up the error budget, but the trade-off is faster growth.
   
   - **Phased rollouts** and **1% experiments** are examples of methods that can help manage risk while using the error budget for quick launches.

5. **Changing the Incentives**:
   - The goal for SREs shifts from **"zero outages"** to managing outages in a way that maximizes feature velocity.
   - **Outages are no longer seen as "bad"**, but as part of the process of innovation. Both the SRE and product development teams collaborate to manage these incidents.

>**Product development teams** are cross-functional groups within an organization responsible for designing, building, testing, and delivering a product. These teams typically consist of members with diverse skills, such as engineers, designers, product managers, and marketers, working collaboratively to create and improve a product throughout its lifecycle. Their goal is to meet customer needs, ensure product quality, and align with business objectives.

>**SRE (Site Reliability Engineering) teams** are responsible for ensuring the reliability, scalability, and performance of software systems in production environments. They bridge the gap between development and operations by applying engineering practices to infrastructure and operations, focusing on automation, monitoring, incident response, and proactive reliability improvements. SRE teams aim to maintain high availability and efficient system performance while balancing the speed of development with operational stability.
## Monitoring in SRE

Monitoring is one of the primary means by which service owners keep track of a system’s health and availability. A thoughtful monitoring strategy is crucial for identifying issues in real-time.

### Common Monitoring Approach

- A classic approach is to monitor a specific value or condition and trigger an email alert when that value exceeds a threshold or when the condition occurs.
- However, relying on email alerts that require human interpretation is not effective. **Monitoring should not require humans to interpret alerts**—the system should handle the interpretation, and humans should only be notified when their action is required.

### Valid Monitoring Output

1. **Alerts**:
   - Alerts signify that a human needs to take immediate action in response to an issue either happening or imminent, to improve the situation.

2. **Tickets**:
   - Tickets signify that action is needed, but not immediately. The system cannot resolve the situation automatically, but a human intervention within a few days is acceptable without causing damage.

3. **Logging**:
   - Logs are for diagnostic or forensic purposes. They are not actively monitored, and no one is expected to look at them unless something else prompts them to do so.
## Emergency Response in SRE

1. **Reliability Metrics**:
   - Reliability is determined by **Mean Time to Failure (MTTF)** and **Mean Time to Repair (MTTR)**.
   - The most important metric for evaluating the effectiveness of emergency response is **MTTR**, which measures how quickly the response team can restore the system to health.

2. **Impact of Humans**:
   - Humans add latency to emergency response. A system that requires fewer human interventions will generally have higher availability.
   - Systems designed to avoid emergencies that require human action will typically outperform those that need hands-on intervention.

3. **Playbooks for Effective Response**:
   - **Playbooks** significantly improve MTTR. A pre-recorded set of best practices can speed up recovery by about **3x** compared to a "winging it" strategy.
   - While smart engineers who can think on the fly are essential, **clear troubleshooting steps** and tips are valuable during high-stakes, time-sensitive situations.

4. **SRE Practices**:
   - Company SRE relies on **on-call playbooks** and **exercises** like the “Wheel of Misfortune” to prepare engineers for effective emergency response.
## Change Management in SRE
1. **Cause of Outages**:
   - Roughly **70% of outages** are caused by changes in a live system.

2. **Best Practices for Change Management**:
   - SRE best practices in managing changes involve automation to achieve the following:
     - **Progressive Rollouts**: Gradually introducing changes to minimize impact on users.
     - **Quick and Accurate Problem Detection**: Identifying issues early to prevent large-scale impact.
     - **Safe Rollbacks**: Rolling back changes promptly and safely when problems arise.

3. **Benefits of Automation**:
   - These practices help minimize the number of users and operations exposed to problematic changes.
   - By removing humans from the loop, the system avoids common human errors such as:
     - **Fatigue**
     - **Familiarity/Contempt**
     - **Inattention to Repetitive Tasks**

4. **Resulting Improvements**:
   - The outcome is a combination of **increased release velocity** and **improved safety** in deployments.
## Demand Forecasting and Capacity Planning

1. **Goal of Demand Forecasting and Capacity Planning**:
   - Ensure sufficient **capacity** and **redundancy** to serve projected future demand with the required availability.

2. **Challenges**:
   - A surprising number of services and teams fail to ensure that the required capacity is in place when needed.
   - The process must account for both **organic growth** (from natural product adoption) and **inorganic growth** (due to events like feature launches or marketing campaigns).

3. **Mandatory Steps in Capacity Planning**:
   - **Accurate Organic Demand Forecast**: Predicting future demand well enough to acquire necessary capacity in time.
   - **Incorporation of Inorganic Demand Sources**: Including business-driven changes (e.g., launches, marketing) in the demand forecast.
   - **Regular Load Testing**: Correlating raw capacity (servers, disks, etc.) with actual service capacity through load testing.

4. **SRE's Role in Capacity Planning**:
   - Since capacity is critical to service availability, the **SRE team** must be responsible for both **capacity planning** and **provisioning**.
## Provisioning

1. **Provisioning Overview**:
   - Provisioning combines both **change management** and **capacity planning**.
   - It must be done **quickly** and **only when necessary**, as **capacity is expensive**.

2. **Risks in Provisioning**:
   - Provisioning involves activities like spinning up new instances, modifying existing systems (e.g., configuration files, load balancers, networking), and validating the performance of the new capacity.
   - These operations are riskier than frequent tasks like **load shifting**, which occurs multiple times per hour.

3. **Best Practices**:
   - **Treat provisioning with extra caution**, given its complexity and the importance of ensuring new capacity works as needed.
## Efficiency and Performance

1. **Importance of Resource Efficiency**:
   - Efficient use of resources is critical when a service cares about its **costs**.
   - **SRE** is responsible for provisioning, and therefore also for managing resource utilization.
   - A well-managed provisioning strategy directly impacts the service’s **total costs**.

2. **Factors Affecting Efficiency**:
   - **Resource use** is determined by:
     - **Demand (load)** on the service.
     - **Capacity** of the system.
     - **Software efficiency**.

3. **SRE's Role**:
   - **SREs** predict demand, provision capacity, and improve software to ensure efficient service operation.
   - As load increases, the system can **slow down**, reducing capacity and efficiency.
   - When a system slows too much, it risks becoming unable to serve requests, equating to **infinite slowness**.

4. **Provisioning and Performance**:
   - SREs target specific response speeds when provisioning to meet **capacity goals**.
   - Both **SREs** and **product developers** actively monitor and modify the service to improve **performance**, increase capacity, and enhance efficiency.
## Managing Risk

1. **Importance of Reducing Failure**:
   - Unreliable systems can erode **users' confidence**, so reducing the chance of failure is a priority.
   - **Cost of reliability** does not increase linearly—improving reliability incrementally can be much more expensive than the previous increment.

2. **Dimensions of Cost**:
   - **Redundant Resources**: Costs associated with additional machine/compute resources and redundant equipment for maintenance, data durability, etc.
   - **Opportunity Cost**: The cost when engineering resources are used to build systems that reduce risk, instead of focusing on user-facing features or new products. 

3. **SRE’s Approach to Risk**:
   - SRE manages service reliability by managing **risk**.
   - **Risk** is treated as a **continuum**, where SREs aim to balance reliability improvements with the appropriate level of risk tolerance for each service.

4. **Risk Alignment with Business Needs**:
   - SRE aligns the level of reliability with the business’s **willingness to bear risk**.
   - Services should be reliable enough to meet business goals but **not overly reliable**, as excessive reliability could waste opportunities for new features or reduce operational efficiency.
   - Availability targets (e.g., **99.99%**) are viewed as **both a minimum and a maximum**, ensuring reliability is optimized without excessive investment.

5. **Goal**:
   - Explicitly balancing **reliability** and **cost**, allowing for thoughtful risk-taking decisions that align with business priorities.
## Measuring Service Risk

1. **Objective Metrics for Optimization**:
   - At Company, we aim to identify an **objective metric** to represent the properties we want to optimize in a system.
   - By setting a **target**, we can assess current performance and track improvements or degradations over time.

2. **Challenges in Measuring Service Risk**:
   - Service failures can lead to multiple impacts: **user dissatisfaction**, **revenue loss**, **brand damage**, and **reputational harm**, which are hard to measure.
   - To make the problem manageable, the focus is placed on **unplanned downtime** as a key measure of risk.

3. **Unplanned Downtime as a Risk Metric**:
   - For most services, **unplanned downtime** is the most straightforward way to represent risk tolerance.
   - Downtime is captured through the desired **service availability**, often expressed in **"nines"** (e.g., 99.9%, 99.99%, 99.999%).
   - The formula for **time-based availability** is:
     ```plaintext
     availability = uptime / (uptime + downtime)
     ```
   - For example, a system with 99.99% availability can be down for up to 52.56 minutes in a year and still meet the target.

4. **Global Service Considerations**:
   - At Company, **time-based availability** metrics are less meaningful due to **globally distributed services** and **fault isolation**.
   - Instead of uptime, **availability** is defined by the **request success rate**, calculated over a rolling window of time:
     ```plaintext
     availability = successful requests / total requests
     ```

5. **Request Success Rate**:
   - For a system serving 2.5 million requests in a day with a target of 99.99% availability, it can afford up to 250 errors in that day.
   - Not all requests are equal—failing a **new user sign-up** is different from failing a **background email polling request**.
   - Availability, based on **request success rate**, is a reasonable approximation of **unplanned downtime** from the **end-user perspective**.

6. **Applicability to Non-serving Systems**:
   - The **request success rate** metric can also apply to **non-serving systems** (e.g., batch, pipeline, storage, transactional systems).
   - In batch systems, availability can be calculated based on the success rate of **records processed**.

7. **Tracking Performance Against Availability Targets**:
   - **Quarterly availability targets** are set for services and performance is tracked **weekly** or **daily**.
   - This allows the team to manage the service to a high-level availability objective, addressing any **deviations** as they arise.
## Risk Tolerance of Services

1. **Defining Risk Tolerance**:
   - The **risk tolerance** of a service refers to the acceptable level of risk or failure the service can endure before it impacts business goals.
   - In formal environments or safety-critical systems, risk tolerance is often defined directly in the product or service's definition.
   - At Company, services’ risk tolerance is not always clearly defined, so **SREs** work with product owners to translate **business goals** into **explicit engineering objectives**.

2. **Consumer vs Infrastructure Services**:
   - **Consumer Services** (e.g., Company Maps, Docs) often have **product teams** responsible for defining risk tolerance and availability requirements.
   - **Infrastructure Services** (e.g., storage systems) typically lack dedicated product teams and must derive risk tolerance through collaboration with engineers.

3. **Identifying Risk Tolerance of Consumer Services**:
   - Factors to consider:
     - **Target availability level**.
     - Impact of different types of failures.
     - Service cost and its relationship to risk tolerance.
     - Other important service metrics (e.g., latency, performance).

4. **Target Level of Availability**:
   - The **availability target** depends on the service's function and market positioning:
     - What do users expect?
     - Does the service tie directly to revenue?
     - Is the service paid or free?
     - What do competitors offer?
     - Is it targeted at consumers or enterprises?
   - **Company Apps for Work**: Typically has high availability targets (e.g., 99.9%) due to its critical role in enterprises.
   - **YouTube**: Initially had a lower availability target due to its rapid growth phase and consumer-focused nature.

5. **Types of Failures**:
   - The **shape of failures** is crucial:
     - **Constant low-rate failures** versus **occasional full-site outages**.
     - **Partial failures** (e.g., rendering issues) may be less impactful than **full outages** or data exposure failures (e.g., private data leaks).
     - **Maintenance windows** may be acceptable for some services, such as the **Ads Frontend**.

6. **Cost Considerations**:
   - Cost is a key factor in determining availability targets, especially when failure translates directly into revenue loss (e.g., in Ads).
   - Cost/benefit analysis helps determine if increasing availability is worth the investment:
     - Example: **Increasing availability from 99.9% to 99.99%** might increase revenue by $900 (based on $1M in service revenue).
   - Services with no direct revenue-to-availability correlation may consider **background error rates** for Internet Service Providers (ISPs), which are typically between **0.01% and 1%**.

7. **Other Important Service Metrics**:
   - **Latency** is another important metric:
     - **AdWords** has a strict latency requirement because it must not slow down the search experience.
     - **AdSense** has a more relaxed latency requirement because it serves ads asynchronously, allowing for greater flexibility in provisioning and reducing operational costs.
   - Different services have different **latency goals**, which influence how they are engineered and provisioned.

>### Summary
>Risk tolerance is a complex and multifaceted concept that depends on service type, business goals, and cost considerations. For consumer services, it's often easier to define, with clear product ownership and expectations. For infrastructure services, the lack of direct product teams requires more collaboration to assess and define appropriate risk tolerance.
## Identifying the Risk Tolerance of Infrastructure Services

Identifying the risk tolerance of infrastructure services involves understanding how different users interact with the service and determining acceptable levels of performance, availability, and reliability based on those needs. The process is more complex than for consumer services because infrastructure components often serve multiple clients with varying requirements.

### Key Considerations for Identifying Risk Tolerance in Infrastructure Services

#### 1. Target Level of Availability
- Infrastructure services like Bigtable serve different types of users with distinct requirements. For instance, some services need low-latency and high reliability, while others prioritize throughput. It's crucial to tailor the infrastructure to meet these needs by offering different service levels.
- For example, Bigtable can have both low-latency and throughput clusters. The low-latency clusters ensure high reliability and short queues for real-time applications, while throughput clusters are optimized for performance with less redundancy and relaxed latency, offering a more cost-effective solution for bulk processing.

#### 2. Types of Failures
- Different failure types have varying impacts depending on the user's needs. For instance, a low-latency user may experience a poor experience if the system queues are long, while a user focused on offline analysis may prefer a system that processes at maximum throughput even if some requests are delayed.
- Failure tolerance can vary greatly. Some services can endure scheduled maintenance outages, while others cannot afford any downtime. The infrastructure must account for these differences by offering different levels of service.

#### 3. Cost Considerations
- The cost of achieving certain levels of reliability or latency is a significant factor in setting risk tolerance. For instance, achieving higher availability often requires more infrastructure resources, which increases operational costs. Services need to weigh these costs against the business needs.
- As an example, Bigtable’s cost for low-latency clusters is much higher than for throughput clusters, but this trade-off allows for more flexibility and cost savings when optimized for different service levels.
   
#### 4. Explicitly Defined Service Levels
- To manage the different requirements effectively, infrastructure services often offer distinct levels of service, each with different performance, reliability, and cost characteristics. For example, critical data might be stored in a highly available, globally consistent datastore, while less important data could reside in a lower-cost, eventually consistent store.
- The key to managing infrastructure risk tolerance is to externalize the difference in costs and service guarantees, enabling clients to select the level of service that best fits their needs while understanding the trade-offs.

> ### Example: Frontend Infrastructure
>- Company’s frontend infrastructure, responsible for handling user requests, must deliver a high level of reliability since failures here directly affect user experience. Ensuring availability and quick response times is crucial, as lost requests cannot be retried once they fail.
>- These systems are designed with high reliability in mind to meet the strict needs of consumer-facing services. However, even in such critical systems, the level of reliability might still vary based on factors like traffic load, redundancy, and infrastructure configuration.

### Conclusion
In summary, identifying the risk tolerance of infrastructure services is about balancing the varying needs of different users (low latency vs. throughput, high reliability vs. cost-effectiveness) and offering flexible service levels that allow clients to make informed decisions about reliability and cost trade-offs. This approach helps manage infrastructure resources efficiently while ensuring that different types of users can rely on the service according to their specific requirements.
## Forming Your Error Budget

To make informed decisions about the amount of risk a service can tolerate, teams use an **error budget**, which is based on the **Service Level Objective (SLO)**. The error budget provides an objective metric that specifies how much unreliability is acceptable within a given time period (usually a quarter). This approach removes subjective negotiations between teams and helps manage service reliability.

### Key Practices for Forming an Error Budget:

1. **Defining the SLO:**
   - **Product Management** defines an SLO, which sets expectations for how much uptime a service should have per quarter.
   
2. **Measuring Uptime:**
   - The actual uptime of the service is measured by a neutral third party, typically a **monitoring system**.
   
3. **Calculating the Error Budget:**
   - The difference between the expected uptime (SLO) and the actual uptime is the **error budget**, which quantifies the allowed unreliability for the quarter.
   
4. **Using the Error Budget:**
   - As long as the actual uptime is above the SLO, meaning there is still error budget remaining, new releases can be pushed.

> #### Example:
>- If a service has an SLO to successfully serve **99.999%** of all queries in a quarter, the error budget allows a failure rate of **0.001%**.
>- If a problem causes a failure of **0.0002%** of the queries, it consumes **20%** of the service’s quarterly error budget.

This structured approach helps teams assess the service's reliability objectively and manage risk appropriately within defined limits.
## Benefits of Error Budgets

The main benefit of an error budget is that it provides a **common incentive** that aligns both **product development** and **SRE (Site Reliability Engineering)** teams to focus on finding the right balance between **innovation** and **reliability**.

### Key Benefits and Practices:

1. **Managing Release Velocity:**
   - Many products use an error budget control loop to manage release velocity. As long as the system’s **SLOs** are met, releases can continue.
   - If **SLO violations** occur often enough to use up the error budget, releases are temporarily halted to invest additional resources in system testing and development to improve resilience, performance, etc.
   
2. **More Subtle Approaches:**
   - More effective techniques than the simple on/off control can be used, such as **slowing down releases** or **rolling them back** when the error budget is close to being used up.

3. **Self-Policing for Product Development:**
   - If product development wants to reduce testing or increase push velocity, but SRE is resistant, the **error budget** serves as a guiding metric for decision-making.
   - When the error budget is large, product developers can take more risks. When the budget is nearly drained, they will push for more testing or slower release velocity to avoid stalling their launch.
   - This leads to product development teams becoming **self-policing**, as they manage their own risk based on the available error budget.

4. **Handling Network Outages or Datacenter Failures:**
   - Events such as network outages or datacenter failures that reduce the measured SLO will also eat into the error budget. As a result, new pushes may be reduced for the rest of the quarter.
   - The entire team supports this reduction, as everyone shares the responsibility for uptime.

5. **Balancing Reliability and Innovation:**
   - The error budget also helps identify the costs of setting overly high reliability targets, which can lead to **inflexibility** and **slow innovation**.
   - If the team faces challenges in launching new features, they may decide to **loosen the SLO** (and thus increase the error budget) to promote more innovation.
## Service Level Terminology

### Indicators (SLI)
An **SLI (Service Level Indicator)** is a carefully defined **quantitative measure** of some aspect of the level of service that is provided.

Common SLIs include:
- **Request Latency**: How long it takes to return a response to a request.
- **Error Rate**: Often expressed as a fraction of all requests received.
- **System Throughput**: Typically measured in requests per second.

SLIs are often aggregated, with raw data collected over a measurement window and then turned into a **rate**, **average**, or **percentile**.

Sometimes only a **proxy SLI** is available, especially if the desired metric is hard to obtain or interpret. For example, **client-side latency** is often more relevant to users, but measuring it may only be possible at the server.

Another important SLI for SREs is **availability**: the fraction of time a service is usable. Availability is often defined as the fraction of well-formed requests that succeed (called **yield**).

Although 100% availability is impossible, near-100% availability is often achievable, and is commonly expressed in terms of the number of **"nines"** in the availability percentage. For example:
- **99%** is “2 nines” availability.
- **99.999%** is “5 nines” availability.
- The current published target for Company Compute Engine availability is **99.95%** or "three and a half nines."

### Objectives (SLO)
An **SLO (Service Level Objective)** is a target value or range of values for a service level that is measured by an SLI. A typical SLO structure is: 

```
SLI ≤ target or lower bound ≤ SLI ≤ upper bound
```
For example, an SLO might specify that the average search request latency should be less than **100 milliseconds**.

Choosing an appropriate SLO can be complex. For example, while you may not control the **queries per second (QPS)** for incoming HTTP requests (which depends on user demand), you can set an SLO for **latency**, which could motivate optimizations like low-latency equipment or frontend design.

Although 100 milliseconds for latency is arbitrary, **fast is better than slow** in general. Studies suggest that user-experienced latency above certain values can drive users away.

### Agreements (SLA)
An **SLA (Service Level Agreement)** is an explicit or implicit contract with users that includes the consequences of meeting or missing the SLOs contained within it. Consequences are often **financial** (e.g., rebates or penalties), but can take other forms.

### Difference Between SLO and SLA:
- **SLO**: Typically doesn’t have consequences if not met.
- **SLA**: Has explicit consequences for missing SLOs (e.g., financial penalties).

SRE teams don’t typically construct SLAs because SLAs are tied to **business and product decisions**. However, SREs do help avoid the consequences of missed SLOs and are involved in defining the SLIs to ensure there is an objective way to measure the SLOs in the agreement.

> #### Example:
>Company Search doesn’t have an SLA for the public, but if it's unavailable, it affects **reputation** and **advertising revenue**. Many other Company services, such as Company for Work, do have explicit SLAs.

Whether or not a service has an SLA, defining SLIs and SLOs is valuable for managing the service.
## Indicators in Practice

### Identifying Meaningful Metrics
Selecting appropriate metrics to measure your service’s performance involves understanding what matters to both **you** and **your users**. Avoid tracking every possible metric—focus on a **handful of representative indicators** to evaluate and reason about the system’s health effectively.

### Key Guidelines:
- **Too Many Indicators**: Overwhelms focus and makes it hard to prioritize.
- **Too Few Indicators**: Risks overlooking significant system behaviors.

### What Do You and Your Users Care About?
Your choice of **Service Level Indicators (SLIs)** should reflect what your users value most. Different types of services prioritize different indicators:

### Categories of SLIs by Service Type:
1. **User-Facing Serving Systems** (e.g., search frontends):
   - **Availability**: Could we respond to the request?
   - **Latency**: How long did it take to respond?
   - **Throughput**: How many requests could be handled?

2. **Storage Systems**:
   - **Latency**: How long does it take to read or write data?
   - **Availability**: Can we access the data on demand?
   - **Durability**: Is the data still there when we need it?
     - *For an extended discussion, see Chapter 26.*

3. **Big Data Systems** (e.g., data processing pipelines):
   - **Throughput**: How much data is being processed?
   - **End-to-End Latency**: How long does it take for data to progress from ingestion to completion?
     - Some pipelines may also set latency targets for individual processing stages.

4. **Correctness** (relevant to all systems):
   - **Key Question**: Was the correct answer returned, the right data retrieved, or the right analysis done?
   - While correctness is critical for system health, it often pertains to the **data** rather than the **infrastructure** itself and may not fall under SRE responsibility.

### Summary
Focusing on the SLIs most relevant to your service type and user needs ensures effective monitoring and better system health management.
## Collecting Indicators

### Server-Side vs. Client-Side Metrics
- **Server-Side Metrics**:
  - Gathered using monitoring systems like Borgmon (see Chapter 10) or Prometheus.
  - Example: HTTP 500 responses as a fraction of all requests.
- **Client-Side Metrics**:
  - Capture user-perceived issues that server-side metrics may miss.
  - Example: Measuring how long it takes for a page to become usable in the browser instead of just the backend response latency.
  - Important when user experience depends on client-side factors (e.g., JavaScript execution).

---

## Aggregation of Metrics
### Challenges in Aggregation:
- **Measurement Windows**:
  - Example: Averaging requests over a minute can hide spikes in instantaneous load.
  - A system serving 200 requests/s in even seconds and 0 in odd seconds appears the same as one with a steady 100 requests/s, despite the burstiness.
  
- **Averages Mask Variability**:
  - Averaging request latencies can obscure long-tail behaviors where some requests take significantly longer.
  - For example, 5% of requests might be 20 times slower than the median latency.

### Distribution Over Averages:
- Metrics should be treated as **distributions** rather than simple averages.
  - Example: Use **percentiles** like 50th (median), 99th, or 99.9th instead of averages.
  - High-percentile values (e.g., 99.9th) indicate worst-case scenarios and provide insight into long-tail behaviors.

### Why Focus on High Percentiles?
- High variance in response times impacts user experience negatively.
- User studies show preference for slightly slower, consistent systems over fast, unpredictable ones.
- Monitoring high percentiles ensures better user experience:
  - If the **99.9th percentile** latency is good, the median is likely fine.

---

## Standardizing Indicators
### Benefits of Standardization:
- Reduces effort by avoiding repeated reasoning about SLI definitions.
- Provides clarity and consistency across teams.

### Components of Standardized SLIs:
- **Aggregation intervals**: E.g., "Averaged over 1 minute."
- **Regions**: E.g., "All tasks in a cluster."
- **Frequency of measurements**: E.g., "Every 10 seconds."
- **Request types**: E.g., "HTTP GETs from black-box monitoring jobs."
- **Data acquisition method**: E.g., "Measured at the server."
- **Latency definition**: E.g., "Time to last byte."

### Reusable Templates:
- Create a set of reusable templates for common SLIs.
- Templates simplify understanding and ensure consistency in the definition and interpretation of SLIs.

---

> ### Summary
>Effective monitoring of indicators requires thoughtful collection, careful aggregation, and standardized definitions. Using distributions and high-percentile values ensures deeper insights into system behavior, while standardized templates save effort and foster consistency.
## Objectives in Practice

### Start with User Needs
- Begin by identifying what **users care about**, not just what is easy to measure.
- Users' needs may be difficult or impossible to measure directly, so **approximate their needs** instead.
- Avoid starting with what’s easy to measure—this often leads to less meaningful SLOs.
- A better approach:
  - Start from **desired objectives** and work backward to identify specific indicators.
  - This method ensures that SLOs align with user priorities.

### Defining Objectives
- SLOs should be clear about:
  - **How they are measured**.
  - **Conditions under which they’re valid**.

#### Examples:
- Full Definition:  
  `99% (averaged over 1 minute) of Get RPC calls will complete in less than 100 ms (measured across all backend servers).`
- Using Defaults:  
  `99% of Get RPC calls will complete in less than 100 ms.`

#### Multi-Target SLOs:
- When performance curve details matter, use multiple SLO targets:
  - `90% of Get RPC calls will complete in < 1 ms.`
  - `99% of Get RPC calls will complete in < 10 ms.`
  - `99.9% of Get RPC calls will complete in < 100 ms.`

#### Heterogeneous Workloads:
- Define separate SLOs for different user needs:
  - **Throughput-focused users**:  
    `95% of throughput clients’ Set RPC calls will complete in < 1 s.`
  - **Latency-focused users**:  
    `99% of latency clients’ Set RPC calls with payloads < 1 kB will complete in < 10 ms.`

# **SLO (Service Level Objective)**

A **Service Level Objective (SLO)** is a specific, measurable target for the performance or reliability of a service, often expressed as a percentage over a defined time period. It represents the expected level of service agreed upon between stakeholders, serving as a benchmark for evaluating system health.

For example: "99.9% of requests should respond within 200ms over the last 30 days."

SLOs are typically derived from **SLAs (Service Level Agreements)** and tracked using **SLIs (Service Level Indicators)** to guide operational priorities and reliability efforts.

## Managing SLO
### Error Budgets
- **100% SLO compliance is neither realistic nor desirable**:
  - Stifles innovation and deployments.
  - Encourages costly, overly conservative solutions.
- Use **error budgets**:
  - Define acceptable rates of SLO violations.
  - Track daily, weekly, or monthly performance.
  - Align deployment processes with error budget consumption.

### Choosing SLO Targets
- **Avoid setting targets based solely on current performance**:
  - Prevents improvement and locks in suboptimal systems.
- **Keep SLO definitions simple**:
  - Complicated SLIs obscure performance trends.
- **Avoid absolutes**:
  - Unrealistic expectations (e.g., "infinite scaling" or "always available") are counterproductive.
- **Minimize the number of SLOs**:
  - Select only those that effectively cover critical system attributes.
  - If an SLO doesn't influence priorities, it likely isn’t valuable.
- **Refine over time**:
  - Start with loose targets and tighten them as system behavior becomes better understood.

### Control Measures
SLOs are integral to system management through control loops:
1. **Monitor and measure** SLIs.
2. **Compare** SLIs to SLOs.
3. **Determine actions** needed to meet targets.
4. **Take action** based on findings.

> Example:
> If latency is rising and threatens to exceed the SLO:
>  1. Hypothesize that servers are CPU-bound.
>  2. Add more servers to balance the load.

### Publishing SLOs
  - **Set expectations** for system behavior:
  - Help users determine whether the system fits their needs.
  - Example:  
    - A photo-sharing website might avoid a service prioritizing durability over availability.
    - However, the same service may suit archival record systems.

### Strategies:
- **Safety Margins**:
  - Use stricter internal SLOs than those advertised to users.
  - Allows flexibility for performance trade-offs, maintenance, or cost optimizations.
- **Avoid Overachievement**:
  - If actual performance exceeds the SLO, users may rely on this higher level of service.
  - Strategies to avoid over-dependence:
    - Introduce planned outages (e.g., Company’s Chubby service).
    - Throttle requests or design the system to handle predictable loads.

### SLOs as Prioritization Tools
- Good SLOs align development efforts with user priorities.
- Poorly defined SLOs lead to:
  - **Wasted work** (overly aggressive targets).
  - **Bad products** (targets too lax).
- SLOs are powerful drivers of focus and resource allocation:
  - Use them wisely to balance system performance, innovation, and user satisfaction.
# SLA: Agreements in Practice

## Crafting an SLA
- **SLA (Service Level Agreement)**: A formal contract that defines consequences and penalties for breaches of service expectations.
- Involves collaboration between **business** and **legal teams**:
  - These teams determine **appropriate consequences and penalties**.
  
### SRE's Role in SLA Development:
- Help business and legal teams:
  - Understand the **likelihood** of meeting the SLOs defined in the SLA.
  - Assess the **difficulty** of achieving the agreed-upon service levels.
- Provide guidance on the technical feasibility and operational implications of the SLA.

### Applying SLO Principles to SLAs
- **Much of the advice for crafting SLOs** applies to SLAs:
  - Ensure clarity, realism, and alignment with user needs.
  - Avoid setting overly ambitious targets that may lead to penalties.

### Being Conservative in SLAs
- Advertised SLAs should err on the side of caution:
  - **Broader constituencies** make SLAs harder to modify or remove.
  - Changes to unwise or overly ambitious SLAs can disrupt user trust and satisfaction.

### Best Practices:
- **Set achievable targets** based on system capabilities.
- **Review periodically** to ensure relevance as system requirements evolve.
# Toil vs Engineering

## What Is Toil?
Toil is **not**:
- "Work I don’t like to do."
- Administrative chores or grungy work:
  - **Overhead**: Tasks like team meetings, setting goals, or HR paperwork.
  - **Grungy work with long-term value**: Example—cleaning up alert configurations.

Toil is work tied to **running a production service** that is:
- **Manual**: Requires human interaction, even if partially automated (e.g., running scripts).
- **Repetitive**: Recurring tasks, not novel problem-solving or invention.
- **Automatable**: Can be replaced by automation or eliminated through design.
- **Tactical**: Reactive and interrupt-driven, not strategy-driven or proactive.
- **Devoid of enduring value**: Doesn’t result in permanent improvement to the service.
- **Scales linearly (O(n)) with service growth**: Increases with traffic volume or user count.

### Why Less Toil Is Better
- **50% Goal**: SREs should spend **less than 50% of their time on toil**.
  - Remaining time should go toward:
    - Reducing future toil.
    - Engineering projects or feature development to improve reliability and performance.
- **Toil expands if unchecked**:
  - Can consume all available time.
  - Reducing toil is essential to scale services **sublinearly** with growth.
- **Promised balance**:
  - SRE is **not an Ops organization**.
  - SRE hires are promised an engineering role, not a purely operational one.

### Calculating Toil
- **Minimum Toil (On-Call Duties)**:
  - A 6-person rotation means 2 out of every 6 weeks (33%) are dedicated to on-call.
  - An 8-person rotation reduces this to 25%.
- **Top Sources of Toil**:
  1. **Interrupts**: Non-urgent service-related messages or emails.
  2. **On-Call Responses**: Urgent service incidents.
  3. **Releases and Pushes**: Still involve manual work despite automation.
- **Company’s SRE Data**:
  - Average toil time: **33%** (better than 50% goal).
  - Outliers exist: Some report 0% (pure engineering) or up to 80% (excessive toil).
  - Excessive toil often requires managers to:
    - Distribute workload evenly.
    - Help SREs focus on satisfying engineering projects.

>### Key Takeaways
>- **Toil must be managed** to ensure time is spent effectively on engineering.
>- **Automation and strategy** are critical tools for reducing toil.
>- **Balancing toil** reinforces the promise of SRE as an engineering-focused discipline.
## What is Engineering?
Engineering work is:
- **Novel**: Requires human judgment and creativity.
- **Strategic**: Guided by broader goals and strategies, not just immediate needs.
- **Permanent**: Produces lasting improvements to systems and processes.
- **Scalable**: Enables handling of more services or larger services without additional headcount.
- **Generalizable**: Solutions are often designed to solve broader problems.

### Categories Work
#### A. Engineering
1. **Software Engineering**:
   - Writing or modifying code, including associated design and documentation.
   - Examples:
     - Writing automation scripts.
     - Creating tools or frameworks.
     - Adding features to improve scalability and reliability.

2. **Systems Engineering**:
   - Configuring production systems or documenting changes for lasting benefits.
   - Examples:
     - Updating monitoring setups.
     - Configuring load balancers.
     - Consulting on architecture and productionization for Dev teams.
#### B. Not Engineering
1. **Overhead**:
   - Administrative work not directly tied to production services.
   - Examples:
     - HR paperwork.
     - Meetings, training, or bug queue hygiene.

2. **Toil**:
   - Manual, repetitive work tied to running a production service (e.g., handling alerts, manually executing scripts).


## Balance Between Toil and Engineering
- **50% Rule**:
  - SREs should spend **at least 50% of their time** on engineering projects.
  - Toil should not dominate over time and should be minimized through engineering solutions.

### Is Toil Always Bad?

#### A. When Toil Is Acceptable:
- Small, predictable amounts of toil can:
  - Be calming and stress-free.
  - Provide a sense of accomplishment with quick wins.
  - Suit individuals who prefer repetitive tasks.

#### B. When Toil Becomes Toxic:
1. **Career Stagnation**:
   - Excessive toil leaves little time for impactful engineering work, limiting career growth.
2. **Low Morale**:
   - Leads to burnout, boredom, and dissatisfaction.
3. **Organizational Impact**:
   - **Confusion**: Dilutes the engineering focus of SRE.
   - **Slowed Progress**: Reduces feature velocity.
   - **Bad Precedents**: Encourages Devs to shift operational burdens to SRE.
   - **Attrition**: Talented engineers may leave for better opportunities.
   - **Breach of Faith**: Betrays the promise of a balanced engineering role for new hires.

## Reducing Toil: A Collective Effort
- Each SRE should commit to:
  - Eliminating small amounts of toil weekly through engineering.
  - Focusing on scalable solutions and strategic improvements.
- Long-term goals:
  - Build services that scale sublinearly with growth.
  - Create tools and frameworks to minimize repetitive tasks.
  - Dedicate efforts to innovative engineering projects, not firefighting.

>**Conclusion**: Let’s invent more and toil less.
# Monitoring Distributed Systems

## Definitions
### Monitoring
- Collecting, processing, aggregating, and displaying real-time **quantitative data** about a system.
- Examples of monitored metrics:
  - Query counts and types
  - Error counts and types
  - Processing times
  - Server lifetimes

### Types of Monitoring
1. **White-box Monitoring**:
   - Based on **internal metrics** exposed by the system.
   - Sources include:
     - Logs
     - Interfaces (e.g., JVM Profiling Interface)
     - HTTP endpoints for internal statistics.

2. **Black-box Monitoring**:
   - Tests **externally visible behavior** as a user would experience it.

### Alert
- A notification pushed to systems like:
  - Bug/ticket queues
  - Email aliases
  - Pagers
- Alert types:
  - **Tickets** (long-term tracking)
  - **Email alerts** (general notifications)
  - **Pages** (immediate attention required)

### Root Cause
- A defect in a **software** or **human process** that, when fixed, prevents the same issue from recurring.
- Incidents can have **multiple root causes** (e.g., process automation failures, crashing software, insufficient testing).

### Node/Machine
- Interchangeable terms for a single instance of a running kernel in:
  - Physical servers
  - Virtual machines
  - Containers
- Machines may host:
  - **Related services** (e.g., caching and web servers)
  - **Unrelated services** (e.g., code repository and configuration system).

### Push
- Any change to a service’s:
  - **Running software** (e.g., deployment).
  - **Configuration** (e.g., infrastructure updates).

## Reasons to Monitor
1. **Analyze Long-Term Trends**:
   - Database size and growth rate.
   - Daily active user count trends.

2. **Compare Over Time/Experiment Groups**:
   - Performance differences between technologies (e.g., DB versions).
   - Impact of changes (e.g., adding a memcache node).

3. **Conduct Ad Hoc Debugging**:
   - Investigate latency spikes or related events.

4. **Build Dashboards**:
   - Visualize core metrics using the **Four Golden Signals**:
     - Latency
     - Traffic
     - Errors
     - Saturation

5. **Alerting**:
   - Notify when:
     - **Something is broken**: Immediate human intervention required.
     - **Something may break soon**: Preemptive action recommended.

## Effective Alerting
- **Purpose**: Enable the system to notify humans when:
  - A problem exists.
  - A potential issue is imminent.
- **Best Practices**:
  - Alerts must not trigger simply because "something seems weird."
  - Prioritize **high signal, low noise** alerts:
    - Reduce unnecessary pages to avoid desensitization.
    - Maintain trust in the alerting system.

## Alerting Costs
- **Human Impact**:
  - Pages interrupt workflows and personal time.
  - Excessive pages lead to alert fatigue, ignored pages, and prolonged outages.

- **Mitigation**:
  - Design alerting systems with meaningful thresholds.
  - Avoid triggering alerts for minor or speculative issues.

## Conclusion
Monitoring and alerting are essential for:
- Detecting system issues in real time.
- Enabling proactive intervention.
- Supporting debugging and system reliability.
# Setting Reasonable Expectations for Monitoring

## Key Takeaways
- **Monitoring is a complex engineering task**, often requiring dedicated team members.
- Company SRE teams typically have at least one member focused on monitoring infrastructure.
- Modern trends favor **simpler, faster systems** with tools for **post hoc analysis** over overly complex or "magic" systems.

---

## Design Principles for Monitoring
1. **Avoid Over-Reliance on Manual Observation**:
   - Monitoring should not require humans to “stare at a screen” for issues.
   - Use automated alerts and dashboards for immediate problem detection.

2. **Keep Rules Simple and Clear**:
   - Rules should detect **unexpected changes** with minimal complexity.
   - Example: Simple rules for end-user request rate anomalies are effective and quick.

3. **Understand System Use Cases**:
   - **Critical Monitoring (e.g., alerting)**:
     - Must be robust and simple to minimize noise.
   - **Secondary Monitoring (e.g., capacity planning)**:
     - Can tolerate higher complexity and fragility.

4. **Limit Dependency Hierarchies**:
   - Complex dependency-based rules (e.g., “if X, then Y”) are avoided.
   - Stable, well-defined dependencies (e.g., datacenter drain rules) are exceptions.

---

## Goals for Monitoring Systems
- Enable quick diagnosis from **symptom** to **root cause**.
- Ensure critical-path components are:
  - Simple
  - Comprehensible by all team members
- Minimize noise while maximizing signal.

---

## **Symptoms vs. Causes**
Monitoring must answer:
- **What’s broken?** (Symptom)
- **Why is it broken?** (Cause)

### Examples
| **Symptom**                            | **Cause**                                              |
|----------------------------------------|-------------------------------------------------------|
| HTTP 500s or 404s                      | Database servers refusing connections.               |
| Slow responses                         | Overloaded CPUs or partial network packet loss.      |
| Users in Antarctica not receiving GIFs | CDN blacklisted client IPs.                          |
| Private content is world-readable      | Software push forgot ACLs.                           |

### Importance of "What" vs. "Why"
- A **good monitoring system** makes clear distinctions between symptoms and causes to reduce noise.
- Example:
  - **Symptom**: Frontend SRE sees slow website performance.
  - **Cause**: Database SRE identifies slow database reads.

---

## Black-Box vs. White-Box Monitoring
| **Aspect**           | **Black-Box Monitoring**                     | **White-Box Monitoring**                |
|-----------------------|---------------------------------------------|-----------------------------------------|
| **Focus**            | Symptoms                                    | Causes and internal system insights     |
| **Use Case**         | Detects current problems.                   | Identifies imminent issues and root causes. |
| **Telemetry**        | Minimal insight into internals.             | Inspects logs, metrics, or endpoints.   |
| **Discipline**       | Pages for ongoing and real problems only.   | Helps distinguish between symptoms and deeper issues. |

---

## Practical Considerations
1. **Telemetry for Debugging**:
   - White-box monitoring is essential for debugging.
   - Example: Distinguish between:
     - Slow database server.
     - Network issues between web servers and the database.

2. **Paging Rules**:
   - Black-box monitoring ensures paging is reserved for active issues with real impact.
   - Avoid using black-box monitoring for imminent, not-yet-occurring problems.

3. **Example for Combined Use**:
   - **Web servers slow on database-heavy requests**:
     - White-box metrics: Database response times and server status.
     - Black-box results: End-user symptoms like HTTP errors.

---

## Conclusion
Effective monitoring balances:
- **Clear symptom vs. cause identification**.
- **Simplicity** in design to avoid overburdening systems or teams.
- **Appropriate use of black-box and white-box monitoring** to ensure reliable alerts and actionable insights.
# The Four Golden Signals

The four golden signals—**latency**, **traffic**, **errors**, and **saturation**—are critical metrics for monitoring user-facing systems. If you can monitor only four things, focus on these.

---

## 1. **Latency**
- **Definition**: The time it takes to service a request.
- **Key Points**:
  - Distinguish between the latency of successful requests and failed requests.
  - Example:
    - A fast error (e.g., HTTP 500 due to a failed database connection) might skew overall latency metrics.
    - Track **error latency** separately to identify patterns like slow errors.
  - **Leading Indicator of Saturation**:
    - Monitor 99th percentile latency over small windows (e.g., 1 minute) to detect potential saturation early.
  - For most services, indirect signals (e.g., CPU or bandwidth utilization) are needed for accurate latency analysis.

---

## 2. **Traffic**
- **Definition**: The amount of demand placed on your system.
- **Measurement**:
  - System-specific metrics:
    - Web service: HTTP requests per second (broken down by request type—static vs. dynamic).
    - Streaming system: Network I/O rate or concurrent sessions.
    - Key-value storage: Transactions and retrievals per second.
  - Monitoring traffic patterns helps correlate demand spikes with issues like errors or saturation.

---

## 3. **Errors**
- **Definition**: The rate of failed requests.
- **Key Points**:
  - Types of failures:
    - Explicit: HTTP 500s.
    - Implicit: HTTP 200s with incorrect content.
    - Policy violations: Responses exceeding defined thresholds (e.g., >1-second latency considered an error).
  - Detection methods:
    - Use **protocol response codes** for explicit errors.
    - Implement secondary checks for more nuanced failure modes (e.g., incorrect content).
    - End-to-end tests are essential for catching implicit errors (e.g., serving the wrong content).

---

## 4. **Saturation**
- **Definition**: How "full" your service is, focusing on constrained resources (e.g., memory, CPU, I/O).
- **Key Points**:
  - Many systems degrade before reaching 100% utilization.
  - Set utilization targets to measure performance under different loads.
  - Use load measurement to assess:
    - Current capacity: How much more traffic the system can handle.
    - Future readiness: Double or reduced traffic scenarios.
  - **Examples**:
    - Predict database storage exhaustion (e.g., "Your database will fill its hard drive in 4 hours").

---

## Practical Considerations

### Latency Analysis: Worrying About the Tail
- Avoid relying on averages for latency metrics:
  - Example: A 100ms average latency might hide significant slow tails (e.g., 1% of requests taking 5 seconds).
- Use **bucketed histograms** for latency distribution:
  - Count requests in latency ranges (e.g., 0–10ms, 10–30ms, 30–100ms).
  - Use exponential bucket sizes for better visualization.

### Paging and Monitoring
- **Latency** and **errors** directly impact user experience and should trigger alerts.
- **Traffic** and **saturation** help diagnose patterns but typically aren't paging candidates unless nearing thresholds.

---

## Conclusion
Focusing on latency, traffic, errors, and saturation provides a comprehensive, actionable framework for monitoring. By prioritizing these signals, teams can ensure a balance between simplicity, accuracy, and actionable insights, enabling proactive issue detection and resolution.
# Choosing an Appropriate Resolution for Measurements

Granularity is critical when measuring different aspects of a system. The resolution should match the needs of the system while balancing cost, complexity, and usefulness.

## Examples of Granularity Choices:
- **CPU Load**: Per-minute measurements may miss spikes that drive high latencies; higher resolution (e.g., per-second) may be needed for detailed analysis.
- **HTTP Status (200)**: Checking more than 1–2 times per minute for a 99.9% uptime target is overkill.
- **Hard Drive Fullness**: Checking every 1–2 minutes suffices for systems targeting 99.9% availability.

## Cost vs. Detail:
- **Frequent Measurements**: Per-second data collection can yield valuable insights but is expensive to store and analyze.
- **Optimization**:
  - Use **internal sampling** to collect high-resolution data (e.g., CPU utilization per second).
  - Aggregate data over time or servers (e.g., into 5% granularity buckets, aggregated every minute).

---

# As Simple as Possible, No Simpler

Monitoring systems must balance complexity with effectiveness.

## Avoiding Over-Complexity:
- Excessive rules and metrics can make systems fragile, hard to change, and a maintenance burden.
- Keep **alert rules simple** and focused on frequent, actionable incidents.

## Guidelines for Simplicity:
- Alerts that are **rarely triggered** (e.g., less than once per quarter) should be reevaluated or removed.
- Metrics not used in dashboards or alerts are candidates for removal.
- Maintain **separate, loosely coupled systems** for monitoring, debugging, profiling, etc., with clear integration points (e.g., web APIs).

---

# Monitoring and Alerting Philosophy

Company's approach provides a foundation for creating effective monitoring systems.

## Key Questions for Alerts:
- **Actionability**:
  - Does this rule detect an **urgent, actionable, and user-visible condition**?
  - Can the alert lead to immediate corrective action, or can it wait?
- **Relevance**:
  - Does the alert **always indicate a problem affecting users**? Filter out test deployments or benign conditions.
- **Response**:
  - Can the response be **automated**, or does it require human intelligence?
  - Will the response provide a **long-term fix**, or is it a workaround?

## Pager Philosophy:
- Respond to pages with urgency; excessive alerts lead to fatigue.
- Pages should:
  - Be **actionable**.
  - Require **intelligent, novel responses** (not robotic or repetitive).
  - Highlight symptoms over causes (except for clear, imminent causes).

## Focus on User Impact:
- Monitoring should prioritize catching symptoms of issues users experience.
- Causes should only be tracked when they are **definite** and **imminent**.

---

# Practical Example: CPU Monitoring

1. **Granular Collection**:
   - Record CPU utilization every second.
   - Increment a bucket (e.g., 0–5%, 6–10%, etc.) for each second.
2. **Aggregation**:
   - Summarize bucket data every minute to balance cost and insight.
3. **Alerting**:
   - Set thresholds for high CPU utilization trends (e.g., 95th percentile over 5 minutes).
# Monitoring for the Long Term

Modern production systems are dynamic, with changing software architecture, load characteristics, and performance targets. As systems evolve, monitoring systems must adapt to track these changes. Alerts that were once rare and difficult to automate may become more frequent, potentially warranting automated scripts to resolve them.

## Long-Term Monitoring Considerations:
- **Root Cause Elimination**: If an alert becomes frequent, it’s important to identify and address its root cause. If that’s not possible, the alert’s response should be automated.
- **Short-Term vs. Long-Term Trade-Offs**: Sometimes, short-term hits to availability or performance are necessary to improve the system in the long term. Every page or alert is a distraction from improving the system for the future.

---

# Conclusion

A healthy monitoring and alerting pipeline should be **simple and easy to reason about**. It should focus on **symptoms** for paging and use cause-oriented heuristics as tools for debugging. 

- **Monitoring Symptoms**: It’s easier to monitor symptoms the further "up" the stack you go. However, for subsystems like databases, monitoring saturation and performance often requires direct monitoring of the subsystem itself.
- **Email Alerts**: These are often noisy and provide limited value. Instead, use **dashboards** to monitor ongoing subcritical problems. Dashboards can also be paired with logs for historical correlation analysis.

## Long-Term Monitoring Goals:
- **Alert on Symptoms or Imminent Problems**: Focus on catching actionable symptoms or imminent issues, not vague or remote causes.
- **Set Achievable Targets**: Adapt your monitoring and alerting targets to realistic and attainable goals.
- **Support Rapid Diagnosis**: Ensure your monitoring system enables quick identification and resolution of problems.

Ultimately, a successful on-call rotation and product depend on choosing the right alerts, aligning targets with achievable goals, and enabling rapid diagnosis to maintain system health over time.
# Release Engineering

Release engineering is a rapidly growing discipline within software engineering, primarily focused on **building and delivering software**. It encompasses a broad set of skills, including an understanding of:
- Source code management
- Compilers
- Build configuration languages
- Automated build tools
- Package managers
- Installers

Release engineers have expertise across multiple domains: development, configuration management, test integration, system administration, and customer support.

## The Importance of Reliable Release Processes

Running reliable services requires **reliable release processes**. Site Reliability Engineers (SREs) need to ensure that the binaries and configurations they use are built in a **reproducible, automated way**. This ensures that releases are repeatable and not "unique snowflakes." Every change in the release process should be intentional, not accidental. SREs are concerned with the entire process, from **source code to deployment**.

## The Role of a Release Engineer

At Company, **release engineering** is a distinct job function. Release engineers collaborate with software engineers (SWEs) and SREs to define all the steps necessary to release software, which includes:
- Storing software in the source code repository
- Defining build rules for compilation
- Managing testing, packaging, and deployment processes

Company is a **data-driven company**, and release engineering is no exception. The company uses tools to track key metrics such as:
- **Release velocity** (how long it takes for code changes to reach production)
- Usage statistics for build configuration files

These tools are often designed and developed by release engineers.

Release engineers also define **best practices** for using these tools to ensure consistent, repeatable releases. These best practices cover various aspects of the release process, including:
- Compiler flags
- Build identification tags
- Required build steps

The goal is to make sure that tools behave correctly by default and are adequately documented, enabling teams to focus on features and user needs rather than reinventing release processes.

## Collaboration Between Release Engineers and SREs

Company has a large number of **SREs** responsible for safely deploying products and maintaining service uptime. Release engineers and SREs work together to:
- Develop strategies for **canarying changes**
- Push out new releases without interrupting services
- Roll back problematic features

This collaboration ensures that the release process meets business requirements while maintaining service reliability.
## Key Points from Release Engineering Philosophy

### Self-Service Model
- Teams must be self-sufficient for scalability.
- Release engineering provides best practices and tools for teams to control and run their own release processes.
- Releases are automated with minimal engineer involvement unless issues arise.

### High Velocity
- Frequent releases help deploy features quickly and improve troubleshooting.
- Teams may perform hourly builds, choosing which to deploy based on test results.
- Some teams use a “Push on Green” release model, deploying builds that pass all tests.

### Hermetic Builds
- Builds are consistent and repeatable across different environments.
- Hermetic builds rely on specific, known versions of build tools and dependencies.
- Older releases can be rebuilt using cherry picks of changes after the original build.

### Enforcement of Policies and Procedures
- Gated operations determine who can perform certain release actions (e.g., code approvals, release creation).
- A code review is required for almost all changes.
- A report is generated detailing all changes in a release for troubleshooting.

### Packaging
- Software is distributed via Midas Package Manager (MPM), which creates packages based on build artifacts.
- Packages are versioned, named, and signed to ensure authenticity.
- Labels are applied to packages to indicate their stage in the release process (e.g., dev, canary, production).

### Rapid System
- **Rapid** is the tool for managing release workflows, supporting large-scale automation.
- Builds are defined and managed with **Blaze**.
- Projects are configured with blueprints to specify the build and release process.
- Workflow actions in Rapid can be parallelized and dispatched using Borg jobs.

### Continuous Build and Deployment Process
1. **Building**: Blaze compiles binaries and runs unit tests.
2. **Branching**: Releases are created from specific branches, not directly from the mainline, to ensure consistency.
3. **Testing**: Continuous test systems catch failures early.
4. **Deployment**: MPM packages are deployed using Rapid, and for complex deployments, **Sisyphus** is used for zero-downtime updates.

### Configuration Management
- Configuration files are packaged with build artifacts for consistency.
- Files can be stored separately but must be versioned alongside the code.
- The goal is zero-downtime deployment, even during configuration changes.
## Key Points from Software Simplicity Philosophy

### System Stability vs. Agility
- Software systems are dynamic and unstable by nature.
- Perfect stability only exists in a vacuum (no code changes, no hardware/library changes, no need to scale).
- **SRE's Job**: Balance stability and agility in the system.
- Sacrificing stability for agility can be necessary, especially for exploratory coding or rapid experimentation.
- **SRE Practices**: Aim to increase reliability without hindering developer agility.
- Reliable systems allow faster bug identification and fixes, improving developer focus on functionality and performance.

### The Virtue of Boring
- "Boring" is a positive attribute for software — it should be predictable, stable, and focused on business goals.
- Surprises in production are undesirable.
- **Essential vs. Accidental Complexity**:
  - **Essential complexity**: Inherent in the problem (e.g., serving web pages).
  - **Accidental complexity**: Arises from poor engineering choices (e.g., performance issues with garbage collection in Java).
- SRE should minimize accidental complexity by eliminating unnecessary complexity in systems.

### Managing Code and Complexity
- **Emotional Attachment to Code**: Engineers may resist purging code, but "dead" code (e.g., commented-out or gated) should be removed to avoid confusion and potential risks.
- **Liability of New Code**: Each new line of code is a potential risk; code should be scrutinized for its essential purpose.
- **Negative Lines of Code Metric**:
  - Software bloat (adding features without removing old code) leads to slower, more error-prone systems.
  - A smaller, cleaner codebase is easier to maintain and test, reducing defects.
  - Removing unnecessary code can be as satisfying as adding new features.

### Minimal APIs
- "Perfection is attained when there is no longer anything to take away" (Antoine de Saint Exupery).
- Simple, clear APIs are crucial to software simplicity.
- Fewer methods and arguments lead to more understandable APIs, allowing focus on improving core functionality.
- A small, minimal API often indicates a well-understood problem.

### Modularity
- **Loose Coupling**: Changes should be made to parts of the system in isolation, enhancing developer agility and system stability.
- **APIs and System Changes**: A single change to an API can require rebuilding the entire system. Proper versioning allows safe upgrades.
- As systems grow more complex, clear modularity between components is essential, similar to object-oriented design principles.
- Avoid "grab bag" classes or "misc" binaries — each system component should have a clear and specific purpose.

### Release Simplicity
- Simple releases are easier to manage and understand.
- **Single Change Releases**: Easier to measure and understand the impact of each change.
- **Batch Releases**: Complicated to track performance issues if multiple changes are released at once.
- Smaller, incremental releases help to identify issues and improve confidence in changes.
- Release strategy can be compared to gradient descent in machine learning: small, deliberate steps to find an optimum solution.
# SRE Practices
## Monitoring
- Monitoring is essential for knowing if a service is working.
- Without proper monitoring, you are unaware of issues until users notice them.
- Thoughtfully designed monitoring is key for proactive issue detection.

## Incident Response
- On-call support is a tool for achieving SRE's larger mission.
- SREs don't carry pagers for the sake of it, but to stay in touch with system performance.
- Effective incident response may involve temporary solutions like graceful degradation or rerouting traffic to a working instance.
- Troubleshooting is key in resolving incidents effectively.

## Development
- SRE teams are heavily involved in system design and software engineering.
- Topics covered include distributed consensus, Cron system, data processing pipelines, and data integrity.
- Chapters cover large-scale systems, distributed periodic scheduling, and maintaining data safety.

## Product
- Once a product is reliable, SRE focuses on launching it effectively at scale.
- Reliable product launches aim to provide users with a good experience from Day Zero.
```
Further Reading from Company SRE
- Resilience Testing: Company conducts company-wide testing to ensure readiness for unexpected events.
- Capacity Planning: It’s vital for long-term stability and doesn’t require predicting the future.
- Network Security: New approach for securing corporate networks through device and user credentials instead of privileged intranets.
```
## Incident Management
- **Emergency Response**: Avoid reacting impulsively during an incident.
- **Managing Incidents**: Effective incident management reduces impact and mitigates anxiety.

## Postmortem and Root-Cause Analysis
- **Blameless Postmortem Culture**: Essential for learning from failures and avoiding repeated issues.
- **Outage Tracker**: An internal tool to track production incidents and responses.

## Testing
- **Testing for Reliability**: Preventing errors before they reach production with proper test suites.

## Capacity Planning and Load Balancing
- **Soware Engineering in SRE**: Case study on automating capacity planning with Auxon.
- **Load Balancing**: Proper distribution of requests across datacenters is critical for reliability.
- **Handling Overload**: Ensuring services are resilient during overload conditions.

## Addressing Cascading Failures
- Strategies for preventing and addressing cascading failures in both system design and during real-world service failures.
## Being On-Call in SRE

### Introduction
- On-call duty is essential for keeping services reliable and available.
- Company’s Site Reliability Engineers (SREs) have developed a unique approach to on-call duties over time, which has led to sustainable workloads and reliable services.

### On-Call in IT Context
- Traditionally, on-call duties were handled by dedicated operations (Ops) teams responsible for maintaining service health.
- In Company, services like Search, Ads, and Gmail are managed by SRE teams, not pure Ops teams.
- SREs are on-call for the services they support, focusing on engineering-driven solutions to operational challenges at scale.

### Key Differences of SRE Approach
- SRE teams emphasize engineering solutions for operational problems, which are too complex to handle without software engineering.
- SREs spend at least 50% of their time on engineering projects, automating processes and scaling impact, rather than purely operational work.
## Life of an On-Call Engineer

### Key Responsibilities
- **Guardians of Production Systems**: On-call engineers manage outages and perform or vet production changes.
- **Paging and Response**: Engineers must respond to alerts within agreed times (e.g., 5 minutes for high-priority systems, 30 minutes for less critical ones).
- **Alert Mechanisms**: Alerts can be sent through multiple devices and mechanisms (email, SMS, app, etc.).
  
### Response Time and Service Availability
- **Service Level Objectives (SLOs)**: If a service requires 99.99% availability, the allowed downtime is about 13 minutes per quarter.
- **Triage and Resolution**: Upon receiving a page, the on-call engineer triages the problem and resolves it, possibly involving other team members.
  
### Non-Paging Events
- **Non-Urgent Alerts**: The on-call engineer may also handle lower-priority alerts or vet software releases during business hours, but paging events take priority.
  
### On-Call Rotation
- **Primary and Secondary Roles**: Teams often have primary and secondary on-call rotations. The secondary may handle missed pages or non-urgent tasks.
- **Fall-Through Rotation**: In some teams, secondary on-call duties may be distributed between teams to reduce the need for an exclusive secondary role.
## Balanced On-Call

### Quantity of On-Call
- **SRE Time Allocation**: SRE teams aim to allocate at least 50% of their time to engineering tasks. Of the remaining time, no more than 25% should be spent on on-call duties, and up to another 25% on other operational tasks.

### Quality of On-Call
- **Incident Management**: An incident is defined as a sequence of events tied to the same root cause, often culminating in a postmortem. Handling an incident, including root-cause analysis, remediation, and follow-up, takes about 6 hours.
- **Incident Limit**: The maximum number of incidents per 12-hour shift is 2. A distribution of incidents should be flat, with a median of 0, to prevent operational overload. If the limit is exceeded temporarily, corrective measures should be taken.
  
### Compensation
- **Incentivization**: On-call compensation can include time-off-in-lieu or direct cash compensation, capped at a certain percentage of salary, which ensures work-life balance and limits burnout.

### Feeling Safe
- **Stress and Cognitive Impact**: Stress hormones can impair decision-making, leading to poor choices during incidents. Intuitive, quick reactions can sometimes be incorrect, so careful analysis is essential.
- **Support Resources**: On-call engineers have access to:
  - Clear escalation paths
  - Well-defined incident-management procedures
  - A blameless postmortem culture
- **Incident Management**: For complex incidents, a formal protocol helps SREs manage the situation effectively. Company's web-based tool automates many aspects of incident management, reducing cognitive load.

### Postmortem and Improvement
- **Postmortem Culture**: SRE teams write postmortems after significant incidents, focusing on events rather than individuals, to improve systems and reduce human error.
- **Automation**: Identifying automation opportunities is key to preventing mistakes in the future.
## Avoiding Inappropriate Operational Load

### Operational Overload
- **Sustainable Workload**: SRE teams aim to allocate no more than 50% of their time to operational tasks. If this limit is exceeded, corrective actions are necessary, including temporarily loaning an experienced SRE to help address the overload.
- **Measurable Symptoms**: Symptoms of operational overload should be measurable (e.g., fewer than 5 daily tickets, fewer than 2 paging events per shift). Misconfigured monitoring is a common cause, and paging alerts must be actionable and aligned with service SLOs to avoid fatigue.

### Operational Underload
- **Risks of Underload**: When SREs are not frequently on-call or a system is too quiet, it can result in overconfidence or underconfidence, leading to knowledge gaps. To prevent this, SRE teams should ensure engineers are on-call at least once or twice a quarter.
- **Training**: "Wheel of Misfortune" exercises and the DiRT (Disaster Recovery Training) event are tools to keep SREs engaged and improve their skills in troubleshooting and disaster recovery.

### Alert Management
- **Reducing Alert Noise**: To prevent overload, it’s essential to manage alert fan-out, ensuring that related alerts are grouped together. Duplicate or unnecessary alerts should be silenced to allow the on-call engineer to focus on resolving the incident.

### Collaboration with Developer Teams
- **Handling Noisy Systems**: If operational overload is caused by changes made by developers, SRE teams should collaborate with them to set goals for improvement. In extreme cases, SREs may temporarily hand over on-call duties to the developer team until the system meets SRE standards.
- **Negotiating On-Call Responsibilities**: In complex cases requiring significant changes, SRE teams can negotiate with developers to share or temporarily transfer on-call responsibilities to ensure the system is sustainable in the long term.

### Conclusion
- **Balanced Approach**: The key to avoiding operational overload and underload is balancing the on-call duties with engineering time. SRE teams at Company follow this model to scale and maintain high service reliability. The relationship between SREs and developers—balancing reliability and feature velocity—results in mutual benefits for the service and the company.
## Effective Troubleshooting

### Theory
- **Hypothetico-Deductive Method**: Troubleshooting follows an iterative process of hypothesizing potential causes for a system failure and testing these hypotheses.
- **Process Overview**:
  - Start with a problem report.
  - Examine system telemetry and logs to understand the current state.
  - Use knowledge of system operation and failure modes to form hypotheses.
  - Test hypotheses by comparing observations with theoretical expectations or by making controlled changes to the system (treating the system).
  - Repeat the process until the root cause is identified and corrective action is taken.

- **Correlation vs Causation**: Correlated events (e.g., packet loss and hard drive failures) may share a common cause, but they don’t imply causation. As systems grow, random correlations can occur, which may mislead troubleshooting efforts.

- **Iterative Hypothesis Testing**: Continuously test hypotheses, either by comparing observations with theory or through controlled experiments, until the root cause is found.

### Common Pitfalls
Ineffective troubleshooting is often caused by problems during the **Triage**, **Examine**, and **Diagnose** steps, typically due to insufficient system understanding.

- **Pitfalls to Avoid**:
  - **Irrelevant Symptoms**: Focusing on symptoms that don’t relate to the problem or misunderstanding system metrics can lead to wild goose chases.
  - **Ineffective Testing**: Misunderstanding how to change the system to test hypotheses can lead to ineffective solutions.
  - **Wildly Improbable Theories**: Jumping to improbable conclusions or assuming past issues are the cause without proper evidence.
  - **Spurious Correlations**: Mistaking coincidences or shared causes for causality.
  
- **Solutions**:
  - **Deep System Knowledge**: Gain experience with system patterns and behavior to avoid misinterpreting symptoms and metrics.
  - **Avoid Logical Fallacies**: Use simpler explanations first (e.g., "When you hear hoofbeats, think of horses, not zebras").
  - **Focus on Probable Causes**: Don’t latch onto unlikely causes based on past events unless supported by evidence.

### Conclusion
A methodical, hypothesis-driven approach, combined with an understanding of common troubleshooting pitfalls, leads to more effective problem-solving. It's essential to test hypotheses and remain open to simpler, more probable causes to avoid wasting time on irrelevant or improbable theories.
## Key Points for Troubleshooting in Complex Systems

### Problem Report
- **Effective Reporting**: 
  - Expected vs. actual behavior.
  - Steps to reproduce the issue.
- **Reporting System**: 
  - Use a bug tracking system to store and manage reports.
  - Standardized forms or automated reporting tools for consistency.
  - Avoid direct reports to individuals to ensure better visibility and workload distribution.

### Triage
- **Severity Assessment**: 
  - Assess impact and severity before deciding on the response.
  - For large outages, prioritize stabilization (e.g., divert traffic, drop load) before root cause analysis.
- **Calm Response**: 
  - Focus on making the system work under the circumstances.
  - Avoid immediately diving into root-cause analysis during emergencies.

### Examine
- **Data and Logs**:
  - Use metrics from monitoring systems and logs to understand system behavior.
  - Tools: Time-series graphs, request tracing, log analysis.
- **Logging Best Practices**:
  - Include multiple verbosity levels.
  - Logs should be structured for both real-time and retrospective analysis.
  - Adjust verbosity on the fly to match investigation needs.

### Diagnose
- **Simplifying and Reducing**:
  - Isolate components to identify where the failure occurs.
  - Test with known inputs and analyze the corresponding outputs.
- **Correlation of Changes**:
  - Link recent changes (e.g., deployments, config changes) to system behavior.
  - Use comprehensive logging for tracking these changes.
- **Test Cases and Black-box Testing**:
  - Reproducible test cases speed up troubleshooting.
  - Use non-production environments for risky testing.
- **Divide and Conquer**:
  - Start from one end of the system and work towards the other.
  - For large systems, use bisection to split the system and test halves iteratively.

### Asking the Right Questions
- **Key Questions**:
  - **What** is the system doing?
  - **Where** are resources being used?
  - **Why** is it doing that? 

### Conclusion
- Structured, methodical troubleshooting prioritizes system stabilization, thorough examination, and careful diagnosis.
- Tools and understanding of system design are crucial for efficient debugging.
## Test and Treat

### Experimental Method for Troubleshooting
- **Hypothesis Testing**: 
  - Form hypotheses about possible causes and test them systematically.
  - For example, test network connectivity or database connection to rule out specific causes.

### Test Design Considerations
- **Mutually Exclusive Alternatives**: 
  - Tests should ideally have clear alternatives to confirm or rule out specific hypotheses.
- **Order of Testing**: 
  - Start with the most likely causes and consider the risks associated with each test.
  - For example, test for network issues before investigating configuration changes.
- **Confounding Factors**: 
  - Be aware of external factors (e.g., firewall rules) that could interfere with test results.
  - Always consider how the environment might affect the outcome.
- **Side Effects of Active Tests**: 
  - Some tests (e.g., changing CPU usage or enabling verbose logging) may alter system behavior in unexpected ways.
  - Understand how a test might affect subsequent results and adjust your approach accordingly.
- **Definitive vs. Suggestive Tests**: 
  - Some tests, like those for race conditions, may be inconclusive or difficult to reproduce.
  - Accept suggestive evidence, but recognize the uncertainty in complex cases.

### Documentation and Tracking
- **Clear Documentation**: 
  - Take detailed notes on the hypotheses, tests performed, and results observed.
  - This is crucial for understanding what happened and avoiding repetition of tests.
- **Systematic Changes**: 
  - If making changes to the system during tests (e.g., adding resources), do so in a documented and reversible way.
  - Ensures that the system can be returned to its original state if necessary.
## Negative Results Are Magic

### Importance of Negative Results
- **Negative Results are Valuable**: 
  - Negative results, or experiments that don’t go as planned, can provide clarity and help resolve complex design questions.
  - They may lead to decisions that guide future efforts, saving time and resources.
  
- **Definitive Nature of Negative Results**: 
  - Negative results offer conclusive evidence about the system, design space, or performance limits.
  - Example: A web server’s failure to handle a required number of connections informs future decisions on server selection.

- **Broader Impact of Negative Results**: 
  - Even if not directly applicable, negative results provide valuable insights that can help avoid past mistakes.
  - Microbenchmarks, documented antipatterns, and postmortems fall into this category.

- **Long-Term Value of Tools and Methods**:
  - Experimenting may produce tools that remain valuable, like benchmarking tools or load generators, which can be used in future projects.

- **Publishing Negative Results**:
  - Share negative results to contribute to a data-driven industry culture. Publishing these results reduces bias and encourages others to accept uncertainty.
  - Don’t shy away from reporting negative outcomes, as they often lead to industry-wide learning.
  - Example: SRE’s high-quality postmortems improve production stability.
  
- **Encouragement of Transparency**:
  - Publishing both successful and unsuccessful experiments encourages a culture of openness and helps everyone learn more quickly.
  - Skepticism towards documents that avoid mentioning failure, as they might be filtered or lacking rigor.

### The Role of Negative Results
- **Learn from Surprising Results**:
  - Share surprising findings, as they can benefit your peers and your future self by avoiding repetition of mistakes.

## Cure

### Proving the Cause
- **Identifying the Root Cause**:
  - After narrowing down the possibilities, it's time to definitively prove the cause. However, this can be difficult due to system complexity.
  - **Challenges**: 
    - Systems are complex with multiple contributing factors.
    - Reproducing the problem in production is often difficult or impractical.

- **System Complexity**:
  - Real systems are path-dependent, meaning a specific state may be needed for the failure to occur.
  
- **Non-Production Environments**:
  - Testing in a non-production environment can help, but this requires maintaining an additional system copy, which adds overhead.

### Postmortem
- **Documenting the Findings**:
  - After identifying and fixing the problem, write a postmortem that details:
    - What went wrong
    - How you tracked the problem
    - How it was resolved
    - Preventive measures for the future
  - This documentation helps in preventing recurrence and facilitates knowledge transfer.
## Making Troubleshooting Easier

### Fundamental Approaches to Troubleshooting
- **Building Observability**:
  - Integrate observability from the start with white-box metrics and structured logs for each component.
  
- **Designing Systems with Observable Interfaces**:
  - Ensure that system components have clear and observable interfaces to simplify troubleshooting.

- **Consistent Information Across the System**:
  - Use a unique request identifier across all RPCs (Remote Procedure Calls) to make it easier to track issues across components and reduce the time spent matching logs from upstream and downstream components.

### Preventing Troubleshooting Needs
- **Correct Representation of Changes**:
  - Problems often arise from misrepresenting the state of reality in code or environment changes.
  - Simplifying and controlling such changes can minimize the need for troubleshooting.

- **Logging and Managing Changes**:
  - Proper logging of changes makes it easier to identify and resolve issues when they occur.
## Emergency Response: What to Do When Systems Break

### Key Principles
- **Stay Calm**: Take a deep breath, remember that you're a professional, and handle the situation calmly.
- **Get Help When Needed**: If overwhelmed, bring in more people—sometimes paging the entire company might be necessary.
- **Follow the Incident Response Process**: Ensure you're familiar with and follow the company’s incident response process.

## Test-Induced Emergency Example
- **Scenario**: A test intended to flush out hidden dependencies by blocking access to one of a hundred MySQL databases resulted in unexpected system failures.
- **Response**: 
  - The test was aborted as soon as the impact was realized.
  - Permissions were restored using pre-tested methods (replicas and failovers).
  - Developers were contacted to fix the issue in the database application layer.
  - Full access was restored within an hour, and services returned to normal.

### Findings: What Went Well
- **Rapid Identification and Response**: The test was aborted immediately, and services escalated issues quickly.
- **Effective Restoration**: Access was fully restored within an hour, and parallel efforts were made to reconfigure systems or restore services.
- **Thorough Follow-Up**: Action items to prevent recurrence were resolved quickly, including the plan for periodic retesting.

### Lessons Learned
- **Insufficient Understanding of Dependencies**: Despite careful review, the scope of the test didn’t fully account for all system interactions.
- **Incident Response Process**: The incident response process was not fully disseminated, causing confusion. Continuous refinement and communication of incident response tools and procedures are critical.
- **Rollback Procedures**: Testing of rollback procedures in a test environment wasn’t done thoroughly, which extended the outage. Going forward, thorough testing of rollback procedures is required.
## Change-Induced Emergency

### Key Scenario
- **Problem**: A configuration change intended to protect services from abuse was pushed globally, causing a crash-loop bug that affected almost all externally facing systems and internal applications.
- **Response**:
  - Alerts fired almost immediately, indicating the crash-loop issue.
  - Some engineers mistakenly thought the corporate network had failed, relocating to panic rooms.
  - The engineer responsible for the change rolled it back within minutes, restoring service.
  - Full recovery occurred within an hour, although some services continued to experience unrelated bugs.

### Findings: What Went Well
- **Monitoring and Alerts**: The problem was detected and alerts were triggered quickly. However, the volume of alerts was overwhelming and disrupted communication.
- **Incident Management**: Incident management went well, with frequent and clear communication and reliable backup systems keeping the team connected.
- **Tools and Access**: Command-line tools and alternative access methods worked well, though engineers could benefit from more familiarity and routine testing of these tools.
- **Infrastructure Protections**: Rate-limiting behavior of the affected system helped to throttle the crash-loop, preventing a complete outage.
- **Swift Rollback**: The push engineer’s quick response to the issue prevented a more prolonged outage and made troubleshooting easier.

### Lessons Learned
- **Canary Testing**: The change had passed an earlier canary test, but the specific configuration keyword combination that triggered the failure hadn’t been tested. This failure raised the priority of improving canarying processes, even for seemingly low-risk changes.
- **Alerting Challenges**: The overwhelming number of alerts made it difficult for on-call engineers to focus and communicate effectively during the incident.
- **Dependence on Internal Tools**: Many troubleshooting and communication tools were unavailable due to the crash-loop, highlighting the need for better redundancy in tools used for diagnostics.

### Key Takeaways
- Thorough canary testing is crucial, regardless of the perceived risk of a change.
- While backup systems were crucial, engineers need to be better prepared and trained in using the tools to mitigate such issues.
## Process-Induced Emergency

### Key Scenario
- **Problem**: A subtle bug in the automation system caused all machines in soon-to-be-decommissioned installations to be sent to the Diskerase queue, wiping their hard drives globally.
- **Response**:
  - On-call engineers received alerts and quickly drained traffic from the affected locations to prevent failures, redirecting it to functional locations.
  - The engineers disabled all team automation to prevent further damage and froze additional automation.
  - Within an hour, all traffic was redirected, and the outage officially ended.
  - The recovery phase began, with engineers working to rebuild the affected servers. Full recovery was achieved within three days.

### Findings: What Went Well
- **Traffic Management**: Traffic was efficiently rerouted from smaller to larger installations, minimizing impact, as large installations could handle full loads.
- **Effective Communication**: Communication and collaboration across teams were exceptional, aiding the swift recovery process.
- **Automation Efficiency**: The turndown automation was fast and effective, with engineers quickly reverting monitoring changes to assess damage.
- **Incident Response**: The company’s matured incident response protocols and trained teams helped manage the situation efficiently.

### Lessons Learned
- **Root Cause**: The turndown automation lacked necessary sanity checks. An empty response was incorrectly passed to the machine database, triggering the Diskerase action for all machines.
- **Reinstallation Issues**: Machine reinstallation was slow and unreliable due to network QoS issues and failures in the BIOS of machines. Engineers prioritized traffic and used automation to restart stuck machines.
- **Infrastructure Limitations**: The machine reinstallation infrastructure struggled with large-scale reinstallation due to a regression that limited task concurrency and had misconfigured QoS settings. Quick action from the responsible teams resolved the issue.
  
### Key Takeaways
- Improved checks in automation could have prevented the widespread issue.
- Better handling of large-scale machine installations and network traffic is necessary to support such high-volume operations.
## All Problems Have Solutions

### Key Insights
- **Solutions Exist**: Problems will inevitably arise, often in unexpected ways. The important thing is that solutions exist, even if they're not immediately obvious. Involving your teammates and acting quickly is key to resolving issues.
- **Utilize Expertise**: The person most familiar with the issue (often the one whose actions triggered the problem) should be closely involved in the resolution. 
- **Post-Incident Reflection**: Once the emergency is resolved, make time to clean up, document the incident, and ensure that lessons are learned to avoid repeating the same issues.

### Learning from the Past: Asking Big Questions
- **Reality Check**: Test your systems' reactions to worst-case scenarios. Ask open-ended questions such as:
  - What if building power fails?
  - What if data centers go dark?
  - What if the web server is compromised?
- **Proactive Testing**: Theory and reality can differ. Don't rely on assumptions—test your systems thoroughly so you know how they’ll react when failure happens. Ensure you have the right people available to address failures when they occur.

### Key Takeaways
- **Incremental Improvement**: Incidents are an opportunity to learn. Responders didn’t panic, they learned from previous outages, and improved their systems over time.
- **Documentation**: Document every failure to create a history of outages, ensuring that your team learns from past mistakes. Postmortems should be detailed, honest, and strategic to prevent similar issues in the future.
- **Accountability**: Follow through on the actions identified in postmortems. Holding the team accountable for learning and improving will help prevent the recurrence of similar outages.
- **Preventive Measures**: Once you've learned from past failures, use this knowledge to proactively prevent future ones.
  
### Conclusion
While each emergency case may have its own unique trigger, the common thread is that effective incident response, learning from failures, and proactive testing lead to continuous improvement. The lessons from Company’s experiences can be applied to organizations of any size to improve both processes and systems.
## Managing Incidents

### Unmanaged Incidents: A Case Study
Imagine being Mary, an on-call engineer for a company, when you suddenly face a cascading failure in your service:
1. **Problem Starts**: Your service stops serving traffic in one datacenter, then another, and eventually all five.
2. **Traffic Overload**: The remaining datacenters can't handle the load, causing further overload.
3. **Troubleshooting**: You begin investigating logs, suspecting an error in a recent module update. Rolling back doesn’t help, so you call Josephine for assistance.
4. **Escalating Pressure**: Your boss and other business leaders demand answers, putting pressure on you while you try to fix the issue. Meanwhile, others offer suggestions that are irrelevant or unhelpful.
5. **Further Complications**: A colleague, Malcolm, decides to implement a quick fix (changing CPU affinity) without consulting anyone, making the situation worse.

### Anatomy of an Unmanaged Incident
Despite everyone trying to do their job, the incident spirals out of control due to several key factors:

#### 1. **Sharp Focus on the Technical Problem**
   - Mary, the on-call engineer, was focused on fixing the technical issue at hand. This prevented her from seeing the bigger picture of the operational needs and the broader impact of the failure.

#### 2. **Poor Communication**
   - Because Mary was overwhelmed with technical tasks, she couldn’t communicate effectively with her team or management. No one knew what actions others were taking, leading to frustration and confusion among both internal teams and external stakeholders.

#### 3. **Freelancing**
   - Malcolm, without consulting Mary or other engineers, made an uninformed change that worsened the situation. Freelancing decisions like these can exacerbate problems instead of solving them.
  
### Key Takeaways
- **Holistic Approach**: Focus on the big picture, not just the technical problem at hand. Ensuring coordination across teams is essential.
- **Clear Communication**: Regular updates and effective communication are vital, especially during an incident. Everyone must be on the same page to work efficiently.
- **Collaborative Effort**: Avoid working in isolation during incidents. Coordination and team involvement can prevent unintended consequences, like escalating the issue further.
## Elements of Incident Management Process

### Overview
A well-designed incident management process is essential to effectively handle incidents. Company's incident management system is based on the [Incident Command System (ICS)](https://en.wikipedia.org/wiki/Incident_Command_System), which emphasizes clarity and scalability. Key features of an effective incident management process include **clear roles**, **delegated responsibilities**, and a well-defined **command structure**.

### Key Features of Incident Management

#### 1. **Recursive Separation of Responsibilities**
   - Clear role separation ensures that each person involved knows their specific responsibilities, reducing overlap and confusion.
   - If workload becomes excessive, team members can request additional staff or delegate tasks, creating sub-incidents if needed.
   - Delegation of responsibilities involves high-level incident management and system-specific actions to ensure efficiency.

#### 2. **Roles in Incident Management**
   - **Incident Commander**: Holds the high-level state of the incident and assigns roles according to priority. They remove roadblocks to help operations work effectively.
   - **Operational Work**: The Ops lead works with the commander to apply operational tools to resolve the incident. The Ops team is responsible for modifying the system during the incident.
   - **Communication**: The public face of the incident response team, handling updates to the team and stakeholders, and keeping the incident document up-to-date.
   - **Planning**: Supports Ops by handling longer-term issues like filing bugs, ordering resources, and tracking system deviations for post-incident resolution.

#### 3. **A Recognized Command Post**
   - Interested parties must know where to interact with the incident commander. A central "War Room" is often used for in-person coordination, but distributed teams can use tools like IRC to stay connected.
   - IRC is valued for its reliability, serving as a communication log during the incident, aiding in postmortem analysis.

#### 4. **Live Incident State Document**
   - The incident commander is responsible for maintaining a live incident document, which serves as a central place for incident tracking.
   - This document should be editable by multiple people simultaneously, often using tools like Company Docs or Company Sites. It's essential for tracking state changes and for later postmortem analysis.
   - A template can help streamline this process and make the documentation easier to follow, with the most crucial information placed at the top for easy reference.

#### 5. **Clear, Live Handoff**
   - When an incident commander’s shift ends, it's essential to have a clear and deliberate handoff to the next commander.
   - The outgoing commander should update the new commander and confirm the handoff with clear acknowledgment, ensuring there’s no ambiguity about who is in charge.
   - The handoff should be communicated clearly to all team members working on the incident to maintain clarity about leadership.

### Key Takeaways
- A structured and clear division of responsibilities ensures smooth incident response and avoids confusion.
- Regular communication and a live document help teams stay aligned and efficient during an incident.
- Proper handoffs and clear leadership at all times are critical for effective incident management.
## A Managed Incident

### Scenario
Mary, the on-call engineer, receives an alert at 2 p.m. that one of the datacenters has stopped serving traffic. She begins investigating, and soon the second datacenter is also down. Knowing the importance of having a structured incident management system, Mary uses the framework to handle the situation effectively.

### Incident Management Process

1. **Initial Response**
   - Mary immediately informs **Sabrina**, asking her to take command of the incident.
   - Sabrina quickly gets a rundown of the situation from Mary and updates the incident status via email to the prearranged mailing list, keeping VPs informed.
   - Sabrina acknowledges that she can't yet scope the full impact and asks Mary for an assessment. Mary estimates that no users are impacted yet and hopes to avoid a third datacenter failure.
   - Sabrina records Mary’s response in a live incident document.

2. **Escalation and Communication**
   - When the third datacenter fails, Sabrina updates the email thread with this new information, keeping the VPs updated without overwhelming them with technical details.
   - Sabrina asks an external communications representative to begin drafting user messaging and reaches out to the developer on-call (**Josephine**) for assistance, after getting approval from Mary.

3. **Collaboration and Documentation**
   - **Robin** volunteers to help, and Sabrina reminds both Robin and Josephine to prioritize tasks assigned by Mary and to keep her informed of their actions.
   - Robin and Josephine read the live incident document to familiarize themselves with the current state of the incident.
   - After testing a previous release fix that didn’t work, Mary shares this with Robin, who updates IRC. Sabrina logs this update in the live incident document.

4. **Shift Change and Handoff**
   - By 5 p.m., Sabrina starts organizing staff replacements for the evening shift and updates the incident document.
   - A phone conference is held at 5:45 p.m. to ensure everyone is on the same page regarding the incident status.
   - At 6 p.m., the incident is handed off to the team in the sister office.

5. **Post-Incident**
   - When Mary returns the next morning, she learns that her colleagues have already mitigated the issue, closed the incident, and started working on the postmortem analysis.
   - Mary uses the lessons learned from the incident to plan structural improvements to prevent similar issues in the future.

### Key Takeaways
- **Clear roles and responsibilities** helped ensure a structured response.
- **Timely communication** kept stakeholders informed without overwhelming them.
- **Collaboration and delegation** ensured the right resources were involved at each stage of the incident.
- **Effective handoff** allowed the incident to be managed across time zones without disruption.

This approach illustrates how an organized, managed incident can lead to quicker resolution and valuable learning for future improvements.
## When to Declare an Incident

### Key Guidelines for Declaring an Incident
It’s crucial to declare an incident early in order to respond more effectively. Delaying the declaration and letting a problem grow can lead to a chaotic response. To determine whether to declare an incident, the following conditions should be considered:

- **Involvement of a Second Team:** Do you need to engage additional teams to help resolve the issue?
- **Customer Impact:** Is the outage visible or impacting customers?
- **Ongoing Issue:** Has the issue remained unsolved despite an hour’s focused analysis?

### Importance of Proactive Incident Management
Incident management proficiency diminishes if it isn’t regularly practiced. To keep skills sharp, engineers can apply the incident management framework to operational changes that span across time zones or involve multiple teams. This ensures familiarity with the process when an actual incident arises.

### Regular Use of Incident Management Framework
- **Change Management:** Use the incident management framework during regular change management procedures.
- **Disaster Recovery Testing:** Include incident management as part of disaster-recovery testing to keep the team prepared.
- **Role-Playing:** Role-play responses to past incidents to enhance familiarity and efficiency with the incident management process.

By integrating incident management into regular operations and testing, teams remain prepared to handle incidents efficiently when they arise.
## Best Practices for Incident Management

### 1. **Prioritize**
   - **Stop the bleeding:** Focus on halting further damage to the system.
   - **Restore service:** Aim to bring the service back online as quickly as possible.
   - **Preserve evidence:** Keep records and logs intact for root-cause analysis.

### 2. **Prepare**
   - **Document procedures:** Develop and document your incident management processes in advance.
   - **Collaborate:** Involve incident participants in the preparation to ensure that procedures are clear and practical.

### 3. **Trust**
   - **Autonomy within roles:** Give incident participants full autonomy within their assigned roles to make decisions and act quickly.

### 4. **Introspect**
   - **Emotional awareness:** Pay attention to your emotional state during an incident.
   - **Seek support:** If you feel overwhelmed or panicked, ask for more help to avoid burnout and ensure a clear response.

### 5. **Consider Alternatives**
   - **Re-evaluate approach:** Periodically step back and reassess if the current strategy is working or if a new approach is needed.

### 6. **Practice**
   - **Routine practice:** Use the incident management process regularly to make it second nature and improve response time and efficiency.

### 7. **Change It Around**
   - **Try different roles:** Rotate roles within the incident management process to gain familiarity with different aspects of the response and improve team coordination.

By following these best practices, teams can respond more effectively to incidents, reduce emotional stress, and improve their ability to handle future incidents.
## Postmortem Philosophy

The primary goals of writing a postmortem are to:
- **Document the incident** to ensure clarity and completeness.
- **Understand all contributing root causes**.
- **Implement preventive actions** to reduce the likelihood and/or impact of recurrence.

While root-cause analysis techniques are extensive and can vary by service, the focus should always be on learning and improvement, not on punishment. Writing a postmortem is a **learning opportunity** for the entire organization.

### Common Postmortem Triggers:
- **User-visible downtime** or degradation beyond a certain threshold.
- **Data loss** of any kind.
- **On-call engineer intervention** (e.g., release rollback, rerouting of traffic).
- **Resolution time above a threshold**.
- **Monitoring failure** (usually implies manual incident discovery).

### Postmortem Criteria:
It’s important to define when a postmortem is required before an incident occurs. Along with the common triggers, any **stakeholder** can request a postmortem.

### Blameless Postmortems:
- **Focus on systemic causes**, not individual mistakes.
- Assume that everyone acted with good intentions and based on the information they had.
- Blameless culture originated in high-stakes industries like healthcare and aviation, where the goal is to learn from mistakes rather than penalize individuals.
- **Encourages transparency and trust**: Individuals are more likely to report issues when they are not afraid of blame.

### Postmortems Should Be Constructive:
- They should identify areas for improvement without finger-pointing. 
- Focus on strengthening systems and processes, not fixing individuals.
  
#### Example of Blameless vs. Blaming Language:
- **Blaming:** "We need to rewrite the entire complicated backend system! It’s been breaking weekly and I'm tired of fixing things.”
- **Blameless:** "Rewriting the backend system could prevent these pages from continuing. The maintenance manual is long and difficult to train on, so future on-callers will likely appreciate a simplified version.”

### Best Practices for Writing a Postmortem:
- **Avoid Blame:** Postmortems should highlight areas of improvement without placing blame.
- **Keep It Constructive:** Focus on solutions and improvements, not frustration.
- **Foster Transparency:** Encourage a culture where incidents are escalated, and people aren’t afraid to report issues.
- **Regular Postmortems:** Frequent production of postmortems should not be stigmatized, as they contribute to long-term improvements.

By embracing blameless postmortems, companies can turn incidents into valuable learning opportunities and improve their systems for the future.
## Collaborate and Share Knowledge

Collaboration is a key aspect of the postmortem process, with a focus on real-time data collection, crowdsourced solutions, and broad knowledge-sharing.

### Postmortem Workflow Features:
- **Real-Time Collaboration:** Enables quick collection of data and ideas during the early stages of creating a postmortem.
- **Open Commenting/Annotation System:** Facilitates crowdsourcing of solutions and improves the coverage of the postmortem.
- **Email Notifications:** Keeps collaborators informed and loops in others to provide additional input.

### Formal Review Process:
- After the initial draft is created, the postmortem undergoes internal review by senior engineers to assess its completeness.
- **Review Criteria Include:**
  - Was key incident data captured for future reference?
  - Are the impact assessments complete?
  - Did the root cause analysis go deep enough?
  - Is the action plan appropriate, with bug fixes prioritized correctly?
  - Was the outcome shared with relevant stakeholders?

Once the initial review is completed, the postmortem is shared more broadly with the larger engineering team or an internal mailing list to maximize learning.

### Best Practice: **No Postmortem Left Unreviewed**
An unreviewed postmortem is as if it never existed. Regular review sessions are encouraged to ensure completeness and closure of discussions. These sessions help capture ideas and finalize the action items.

### Postmortem Repository:
- Once the postmortem is finalized and reviewed, it is added to a team or organization repository for easy access.
- Transparent sharing makes it easier for others to learn from past incidents, ensuring that valuable lessons are not lost.

Incorporating a thorough and collaborative review process ensures that postmortems are effective, and their learnings are widely shared and applied across teams.
## Introducing a Postmortem Culture

Creating a postmortem culture in an organization requires continuous cultivation and reinforcement. Senior management plays a vital role in encouraging and participating in the postmortem process, but ultimately, it is the engineers who must be self-motivated to embrace a blameless postmortem culture.

### Key Activities to Reinforce Postmortem Culture:
- **Postmortem of the Month:** A well-written postmortem is featured in a monthly newsletter, shared across the organization.
- **Company+ Postmortem Group:** A group dedicated to sharing and discussing internal and external postmortems, best practices, and commentary.
- **Postmortem Reading Clubs:** Teams host regular sessions where an impactful postmortem is discussed openly with participants and non-participants.
- **Wheel of Misfortune:** A role-playing exercise where new SREs reenact a past postmortem to simulate real-life scenarios, helping them learn and prepare for future incidents.

### Overcoming Challenges:
Introducing postmortems can face resistance, often due to the perceived cost of preparation. The following strategies help:
- **Ease postmortems into the workflow:** A trial period with several successful postmortems can help prove their value.
- **Reward and celebrate writing effective postmortems:** Recognize individuals and teams publicly for their contributions to postmortems.
- **Encourage senior leadership participation:** Acknowledgment from senior leadership, including the company’s founders, adds credibility to the importance of postmortems.

### Best Practices:
- **Visibly Reward People for Doing the Right Thing:** Recognition can come from peer bonuses, public applause, and social networks, highlighting good postmortem practices and handling of incidents.
- **Ask for Feedback on Postmortem Effectiveness:** Regular surveys help teams provide feedback on how the postmortem process can be improved and how it supports their goals.

### Cultural Integration:
Postmortems are now an integral part of Company's culture, ensuring that any significant incident is followed by a comprehensive postmortem. This cultural norm ensures continuous improvement in the organization’s incident response and system resilience.
## Testing for Reliability

Site Reliability Engineers (SREs) play a key role in quantifying confidence in the systems they manage. They do so by applying classical software testing techniques at scale to assess both **past** and **future reliability**.

### Measuring Reliability:
- **Past reliability** is determined by analyzing historical system data and behavior.
- **Future reliability** is estimated by predicting future system behavior based on past data. This requires one of the following:
  - The system remains unchanged over time (no software releases or changes).
  - Changes to the system are well-documented so that predictions can account for their impact.

### Testing to Reduce Uncertainty:
Testing is used to demonstrate equivalence before and after a change, thereby reducing uncertainty. Thorough testing helps predict the future reliability of a system, and the level of testing required depends on the system's reliability requirements.

### The Relationship Between Testing and System Changes:
- As the percentage of code covered by tests increases, the uncertainty about the system's reliability decreases, allowing more changes to be made without falling below acceptable reliability levels.
- If too many changes are made quickly, the predicted reliability may dip below the acceptable limit, requiring a pause in changes while new monitoring data is collected to validate the reliability of the updated system.

### Testing and Mean Time to Repair (MTTR):
- A test passing does not guarantee reliability, but **failing tests** indicate a lack of reliability.
- **MTTR** measures the time it takes for the operations team to fix a bug (through rollback or other actions). 
- A testing system that detects bugs with zero MTTR can prevent problematic code from reaching production by blocking the push, though the bug still needs to be fixed in the source code. The higher the zero MTTR bugs detected, the higher the **Mean Time Between Failures (MTBF)** for users.

### The Impact of Testing on Release Velocity:
- As **MTBF** increases through better testing, developers are encouraged to release features more quickly.
- New bugs may arise with faster releases, which can impact release velocity as bugs are identified and fixed.

### Testing Terminology:
- Most conflicts in software testing practices arise from differing opinions on coverage requirements, terminology, and the phase in the software lifecycle in which testing occurs.
# Types of Software Testing

Software tests can be divided into **traditional** and **production** tests, each serving distinct purposes in the software development lifecycle.

## Traditional Tests
1. **Unit Tests**
   - Test the smallest, isolated unit of software (e.g., functions, classes).
   - Ensure correctness and can act as a specification for expected behavior.
   - Cheap and fast (run in milliseconds).
   - Often used for test-driven development.

2. **Integration Tests**
   - Test interactions between components or units.
   - Verify that combined components function correctly.
   - Often use mocks for dependencies (e.g., mock databases).

3. **System Tests**
   - Test the entire system, verifying end-to-end functionality.
   - Types of system tests:
     - **Smoke Tests**: Basic checks to ensure critical functionality works.
     - **Performance Tests**: Ensure the system's performance remains stable over time.
     - **Regression Tests**: Ensure that previously fixed bugs do not reappear.

## Production Tests
1. **Configuration Tests**
   - Verify that the production system is correctly configured.
   - Run outside the test environment, ensuring the system matches expected configurations.
   - Essential in distributed environments where production and development configurations may differ.

2. **Stress Tests**
   - Push the system beyond its normal operating limits.
   - Help understand system limits (e.g., database load, server overload).
   - Identify failure points under extreme conditions.

3. **Canary Tests**
   - Gradually roll out changes to a small subset of users or servers.
   - Acts as a form of user acceptance testing in a live environment.
   - Allows early detection of faults before full deployment.
   - Enables quick rollback if issues are detected.

## Key Concepts
- **Production vs. Hermetic Testing**: Production tests run in live environments, unlike traditional tests which occur in controlled settings.
- **Rollout Strategy**: Changes are deployed gradually (e.g., canary tests, staged rollouts) to manage risks.
- **Test Costs**: Traditional tests (like unit tests) are inexpensive, but production tests (like system tests or stress tests) are more resource-intensive.
- **Error Detection in Production**: Production tests help identify issues that wouldn't be caught in isolated tests, particularly when interacting with real traffic or real-world conditions.
# Creating a Test and Build Environment

When SREs join an already in-progress project, comprehensive testing may not yet be in place. In such cases, prioritize efforts to build testing incrementally.

## Steps to Begin Testing
1. **Prioritize the Codebase**:
   - Rank components of the system by importance to identify the most critical areas to test.
   - Example: Mission-critical or business-critical code (e.g., billing) should be prioritized.

2. **Focus on APIs**:
   - Test APIs that other teams rely on, as issues in these areas can cause significant downstream problems.

3. **Smoke Tests**:
   - Implement low-effort, high-impact smoke tests for every release to catch obvious issues early.
   - This helps in shipping reliable software and improving testing culture.

4. **Bug Reporting as Test Cases**:
   - Convert every reported bug into a test case. Initially, these tests will fail until the bug is fixed, contributing to a growing regression test suite.

## Establishing Testing Infrastructure
1. **Source Control**:
   - Set up a versioned source control system to track all changes in the codebase.
   
2. **Continuous Build System**:
   - Add a continuous build system that builds software and runs tests on every code submission.
   - Ensure the system notifies engineers immediately if a change breaks the project, allowing the team to prioritize fixing the issue.

3. **Importance of Fixing Broken Builds**:
   - Treat defects in broken builds with high priority because:
     - It's harder to fix if more changes are made after the defect.
     - Broken software slows down development.
     - Release cadences lose value.
     - Emergency releases become more complex.
   
4. **Stability and Agility**:
   - A stable and reliable build system fosters agility. Developers can iterate faster when the system is stable.

## Tools for Optimizing Build Systems
- **Bazel**:
   - Bazel provides precise control over testing by creating dependency graphs.
   - Only rebuilds parts of the software dependent on changed files, improving speed and reducing cost of tests.

## Goal-Oriented Testing
- **Quantifying Test Coverage**:
   - Use tools to quantify and visualize the required test coverage.
   - Set clear goals and deadlines instead of ambiguous statements like "We need more tests."

## Considerations for Different Software Types
- **Test Quality and Coverage**:
   - Life-critical or revenue-critical systems require higher levels of test quality and coverage compared to less critical software (e.g., short-lived scripts).
# Testing at Scale

In order to drive reliability at scale, SRE takes a systems perspective to testing, addressing dependencies and selecting effective testing environments.

## Key Concepts of Testing at Scale

1. **Unit Tests and Dependencies**:
   - A small unit test typically has a limited set of dependencies (e.g., source file, testing library, runtime libraries, hardware).
   - These dependencies should each have their own test coverage to ensure comprehensive testing.
   - If a unit test relies on an unchecked runtime library, unrelated changes in the environment could lead to the test passing despite faults in the code under test.

2. **Release Tests and Transitive Dependencies**:
   - A release test often depends on many components, potentially having a transitive dependency on every object in the code repository.
   - If the test requires a clean copy of the production environment, each small patch might necessitate a full disaster recovery iteration.

3. **Branch Points in Testing**:
   - Testing environments aim to select branch points in versions and merges to minimize uncertainty.
   - This reduces the number of iterations needed to test and ensures that faults are detected more efficiently.

4. **Resolving Faults**:
   - When uncertainty resolves into a fault, additional branch points need to be selected for further testing.
# Testing Scalable Tools

## SRE Tools and Their Testing Needs

SRE-developed tools are responsible for various tasks, such as:
- Retrieving and propagating database performance metrics
- Predicting usage metrics to plan for capacity risks
- Refactoring data within a service replica that isn’t user accessible
- Changing files on a server

### Characteristics of SRE Tools:
- Their side effects remain within the tested mainstream API.
- They are isolated from user-facing production by an existing validation and release barrier.

## Barrier Defenses Against Risky Software

Software that bypasses the usual heavily tested APIs can create significant risks, especially when interacting with live services. For example, a database engine may allow administrators to turn off transactions, which could cause issues if the utility is run on a user-facing replica.

### Strategies to Mitigate Risk:
1. Use a separate tool to place a barrier in the replication configuration, ensuring the replica doesn’t pass its health check and is not released to users.
2. Configure the risky software to check for this barrier upon startup, limiting its access to unhealthy replicas.
3. Use the replica health validating tool to remove the barrier when needed.

## Testing Automation Tools

Automation tools in SRE are responsible for tasks such as:
- Database index selection
- Load balancing between data centers
- Shuffling relay logs for fast remastering

### Characteristics of Automation Tools:
- They perform operations against a robust, predictable, and well-tested API.
- The operations result in side effects that are invisible to other API clients.

### Challenges in Testing Automation Tools:
- It's crucial to test the internal state before and after operations, ensuring that API invariants hold.
- Certain operations, like DNS cache behavior, may not hold due to changes in the environment (e.g., replacing a local nameserver with a caching proxy).
- Automation tools need to be tested in the specific environments they are designed to work in, ensuring they don't conflict with each other (e.g., container rebalancing and fleet upgrading tools might conflict if not managed correctly).

### Example of Automation Tool Interactions:
- Automation tools can influence each other’s environments, such as when a fleet upgrading tool uses resources that impact a container rebalancing tool, which may, in turn, affect the fleet upgrading process.
- Circular dependencies between tools are acceptable if APIs have restart semantics and are properly tested for recovery and health.
# Testing Disaster

## Disaster Recovery Tools

Many disaster recovery tools are designed to operate offline and are used for:
- Computing a checkpoint state that represents a clean stop of the service.
- Pushing the checkpoint state to be loadable by existing nondisaster validation tools.
- Supporting the usual release barrier tools, which trigger the clean start procedure.

### Benefits of Offline Tools:
- These tools are easier to test and offer excellent coverage when the constraints (offline, checkpoint, loadable, barrier, clean start) are maintained.
- If any of these constraints are broken, it's harder to ensure the tool will work reliably on short notice.

## Online Repair Tools

Online repair tools operate outside the mainstream API and require more complex testing. One of the challenges is ensuring that normal behavior (eventual consistency) interacts well with the repair process.

### Challenges with Online Repair Tools:
- **Race Conditions:** These are harder to test because the repair tool is often built separately from the production binary.
- **Consistency:** Offline tools expect instant consistency, while online repairs may deal with eventual consistency, which complicates testing.
- **Unified Instrumentation:** You may need to build a unified instrumented binary to run tests that allow both the production and repair tools to be observed during transactions.

## Using Statistical Tests

### Statistical Testing Techniques:
- **Tools:** Techniques like Lemon (for fuzzing), Chaos Monkey, and Jepsen (for distributed state) are not repeatable tests but can still be useful.
- **Logs:** These tools can log all actions taken during a run, sometimes by logging the random number generator seed.
- **Refactoring Logs:** The log of actions can be refactored into a release test. Running it before bug fixing helps determine how difficult it will be to assert that the fault is fixed.
- **Fault Expression:** Variations in how a fault is expressed can help pinpoint issues in the code.
- **Severity Escalation:** Subsequent test runs may reveal more severe failure situations, leading to an increase in bug severity and impact.
# The Need for Speed

## Test Likelihood and Statistical Uncertainty

- **Test Pass/Fail Indication:** Each version in the code repository provides a pass or fail indication for every defined test. These indications can change over repeated runs.
- **Statistical Estimation:** To estimate the likelihood of passing or failing, you would typically average over many runs and compute statistical uncertainty. However, calculating this for every test at every version is computationally infeasible.
- **Hypothesis Formation:** Engineers must form hypotheses about scenarios of interest and run enough tests to make reasonable inferences. Some scenarios are benign, while others are actionable.
- **Actionable Hypotheses:** Engineers aim to identify components that are actually broken, especially when their code isn't the cause of failure. This helps distinguish flaky tests caused by external factors versus genuine issues in the engineer's code (e.g., unanticipated race conditions).

## Testing Deadlines

- **Simple Tests:** Simple tests usually run in seconds, providing immediate feedback for engineers, which helps them avoid switching contexts frequently.
- **Complex Tests:** Tests requiring orchestration across many binaries or containers tend to have longer startup times and may not provide interactive feedback. These are classified as batch tests.
- **Feedback Timing:** The informal deadline for tests is before the engineer switches to another task. If the feedback comes too late, the engineer may lose focus and switch to something else.
  
### Release Testing Process:

- **Release Test Vector Comparison:** Engineers compare the pass/fail results from before and after a patch to assess whether the patch is safe for release.
- **Scaling and Complexity:** Release and integration tests check the impact of patches on system scaling and workload complexity.
- **Environmental Flakiness:** Miscalculating environmental flakiness can result in incorrectly flagging a patch as damaging. The ideal error rate for such miscalculations is extremely low (over 99.9999%).

### Error Rate Calculation:

- **Error Rate Formula:** The error rate for patch rejection can be calculated using the formula:
  \[
  0.99^{(2 \times 21,000)}
  \]
  which suggests that each individual test must pass with 99.9999% accuracy to avoid incorrect rejection of patches.
# Pushing to Production

## Segregation of Testing Infrastructure and Production Configuration

- **Separation Issues:** Production configuration is often managed separately from developer source code, sometimes even in different repositories or directories. Additionally, testing infrastructure often doesn't have visibility into the production configuration.
- **Legacy Environment Risks:** In legacy environments, where software engineers develop binaries and pass them off to administrators for server updates, the separation of configuration and testing infrastructure can be problematic:
  - **Annoying at best, damaging at worst:** It hinders reliability and agility and can lead to duplicated tools.
  - **Degrades resiliency:** In integrated Ops environments, this segregation leads to inconsistencies between the behavior of the two sets of tools, weakening the system's resilience.
  - **Limits project velocity:** The segregation also creates commit races between versioning systems, slowing down development.

## SRE Model and the Impact of Segregation

- **SRE Model Challenges:** In the SRE model, the separation of testing infrastructure from production configuration is even more damaging. It prevents correlating production behavior models with application behavior models.
- **Impact on Development:** This segregation hinders engineers' ability to detect statistical inconsistencies during development, impacting their ability to ensure system reliability.
- **System Architecture Constraints:** The segregation doesn’t just slow down development—it can prevent changes in system architecture, as migration risks remain unmanaged.

## Testing Distributed Architecture Migration

- **Migration Risk:** If a migration in a distributed architecture fails, some testing will likely occur, but there's a risk that testing may return a false negative. The potential for an error to go undetected could escalate the situation quickly.
- **Higher Risk Areas:** Some areas of the system require more rigorous testing coverage than others. It's important to identify which test failures could have a larger impact and require more attention and "paranoia" to mitigate risks effectively.
# Expect Testing Fail

## Evolution of Release Cadence

- **Historical Releases:** In the past, software releases were annual events, with slow and manual testing processes. The reliability of these releases (Mean Time Between Failure or MTBF) was relatively low, and the tests often couldn’t uncover all breakage.
- **Modern Release Cycle:** With the advent of faster build tools and automated testing, new versions can be released much more frequently. By testing intermediate versions, problems can be mapped to their causes, improving the overall quality of each release.
  - **Shorter Feedback Cycle:** A quicker feedback loop enables better identification of issues and causes, improving overall software quality over time.
  - **Discovery of Gaps in Test Coverage:** More frequent releases may expose user-facing breakage, but also highlight areas that need additional test coverage. Addressing these gaps can enhance future reliability.

## Reliability Management in SRE

- **Reliability Budget by Functionality:** The release cadence can be segmented by functionality or team, with each team having a different reliability budget. The SRE team's job is to ensure the production environment runs smoothly, typically in an unattended manner, requiring resilience against minor faults.
- **Major Faults and SRE Tools:** When a major failure happens, SRE tools must be well-tested to maintain confidence in the system. If these tools aren’t tested properly, manual intervention can lower confidence in the system’s reliability.

## Configuration Management

- **Configuration File Challenges:** Configuration files are often modified to keep the mean time to recovery (MTTR) low. However, if these files are edited too frequently, they can introduce significant risks if not adequately tested.
  - **Slow Release Cadence for Configuration Files:** Configuration files that are edited infrequently should have slower release cadences. These files need more testing to ensure the changes are optimal for overall reliability.
  - **Frequent Changes:** If configuration files are modified with each user-facing application release, the changes must be treated with the same rigor as application releases. Inadequate testing or monitoring of these files can negatively affect site reliability.
  
## Handling Configuration Files

- **Categorizing Configuration Files:** Ensure each configuration file is categorized based on its release frequency and reliability requirements:
  - **Routine Editing:** Configuration files that are regularly edited should have enough test coverage to support these changes.
  - **Pre-release Testing:** Delay file edits before releases to allow for proper testing. A "break-glass" mechanism should be in place for urgent manual edits.
  - **Break-Glass Mechanism:** This mechanism allows urgent changes to be pushed live before complete testing. However, it should trigger a bug filing process and increase the priority of subsequent release tests. This ensures that manual changes don’t undermine reliability.

## Conclusion

- **Testing Failures and Impact:** Testing failures are inevitable, but the goal is to manage them with proper coverage, especially for critical configurations, to minimize user impact.
- **Reliability Focus:** Each edit or change, particularly in configuration files, needs careful testing, with mechanisms to quickly recover and mitigate the impact of potential failures.
# Integration Testing for Configuration Files

## Risks of Configuration Files in Interpreted Languages

- **Interpreted Languages for Configuration:** Using interpreted languages like Python for configuration files can introduce risks. These languages are commonly used because their interpreters can be embedded and allow for simple sandboxing. However, these files may cause latent failures that are difficult to address definitively.
  - **Inefficiency in Loading:** Loading a configuration file involves executing a program, which can lead to inefficiencies without an inherent upper limit on loading time.
  - **Integration Testing:** Integration testing should include careful time limits. If tests don't complete within a reasonable time, they should be marked as failures.

## Risks of Custom Syntax and Text-Based Configuration Files

- **Custom Syntax Risks:** If configuration files are written in a custom syntax, each category of test needs its own dedicated coverage. This can lead to more complex and error-prone testing processes.
- **YAML and Safe Parsers:** Using existing syntaxes like YAML and a well-tested parser like Python's `safe_load` can simplify the process, providing some bounds on runtime and reducing the risk of inefficiencies. However, it still requires careful handling of schema faults.
- **Protocol Buffers for Schema Validation:** Using Protocol Buffers (protobufs) ensures that the schema is predefined and automatically checked during the load process, further reducing the risk of runtime inefficiencies and simplifying validation.

## Role of Site Reliability Engineering (SRE)

- **SRE's Role in Tooling:** SREs often write systems engineering tools and add robust validation and test coverage to ensure reliability. Defense in depth is crucial, as bugs or unexpected behavior in one tool can lead to issues in others.
  - **Tool Failures and Misbehavior:** Even with testing, tools can fail. It’s essential to have additional tools in place to detect misbehavior and report issues before they cause outages.
  
## Example of Configuration Failure

- **Example Scenario:** Consider a configuration like `/etc/passwd` on a Unix-style machine. If an edit causes the parser to stop after reading only part of the file, some users may be missing. The system may continue functioning, and users might not notice the issue immediately.
  - **Tool for Detection:** A tool that maintains user home directories can detect the mismatch between actual directories and those implied by the partial configuration. This tool should report the issue without attempting to fix it on its own, as doing so could lead to catastrophic errors (e.g., deleting user data).

## Conclusion

- **Integration Testing Importance:** Whether using an interpreted language or a custom syntax, integration testing for configuration files is critical to ensure they behave as expected without introducing inefficiencies or errors. 
- **SRE's Role in Reporting:** SREs should ensure that tools designed to detect misbehavior can alert engineers to issues early, preventing major outages while avoiding automatic remediation actions that could worsen the situation.
# Production Probes

## Risks Beyond Testing and Monitoring

While testing ensures known data behaves correctly, and monitoring confirms behavior in the face of unknown user data, actual risk management is more complex. It requires coverage for:
- **Known good requests** (requests expected to work)
- **Known bad requests** (requests expected to fail)
- These should be implemented as integration tests, and replaying these test requests as a release test helps in validating the software.

## Three Sets of Requests

When dealing with different software versions (older and newer), there are three types of requests:
1. **Known bad requests** (expected to fail)
2. **Known good requests that can be replayed against production**
3. **Known good requests that can’t be replayed against production** (these need their own testing)

### Challenges with Different Versions
- Components may interact with different versions (older software using deprecated APIs, newer versions interacting with older APIs that weren’t fully functional).
- Monitoring probes must ensure that future compatibility is covered, ideally with good API coverage.

## Fake Backend Versions in Release Testing

- **Fake Backend Maintenance:** Release tests often use fake backends maintained by the peer service’s engineering team, ensuring that the frontend and backend tests are hermetically integrated.
  - **Hermetic Testing:** The backend and frontend are tested together in a controlled environment to ensure compatibility, ideally with synchronized release schedules.
  - **Backend and Frontend Releases:** Testing should be aware of all release versions for both components and run new combinations when either side releases a new version.
  
## Monitoring and Rollout Automation

- Monitoring should track all release versions on both sides of a service interface, ensuring that new combinations are tested without requiring excessive configuration.
- **Rollout Automation:** Production rollouts should be blocked if problematic combinations of releases are detected, with the peer team's automation ensuring that replicas are upgraded to avoid issues.

## Role of Monitoring Probes

- **Deployment of Monitoring Probes:** Probes in production ensure that the system behaves as expected, even though the release tests have already covered similar scenarios.
- **Production Probes Configuration:** Production probes are different because they wrap the release binary with load balancing and persistent backend services, which often have different release cycles.
- **Purpose of Probes:** These probes help detect discrepancies between the production and release environments, alerting engineers if the APIs in either the frontend or backend are not equivalent.

## Handling Failures in Probes

- If probes fail, it indicates that the production and release environments are not in sync, which could point to a broken system unless the issue is already known.
- **Production Updater:** The updater gradually replaces application and probe versions, continuously testing all combinations. If an error occurs, the updater rolls back to the last known good state, preventing issues from affecting users.

## Benefits of Production Probes

- **Early Detection:** Probes provide feedback early, allowing engineers to address issues before they impact users.
- **Automated Monitoring:** Automated probes allow scalable, continuous feedback to engineers, ensuring production stability.

## Conclusion

- Production probes act as a safeguard to protect the site from issues during release transitions. By detecting failures early and rolling back to a stable state, they provide valuable feedback for engineers while preventing user-facing problems.

---
# Glossary
| Term                        | Definition                                                                 |
|-----------------------------|---------------------------------------------------------------------------|
| **Availability**             | The proportion of time a system is operational and accessible.            |
| **SLO (Service Level Objective)** | A target level of service for a given metric, often expressed as a percentage. |
| **SLA (Service Level Agreement)** | A formal agreement that outlines the expected level of service between parties. |
| **SLI (Service Level Indicator)** | A metric that measures the level of service provided.                    |
| **Error Budget**             | The amount of downtime or errors a service is allowed within a given period while still meeting the SLO. |
| **Incident**                 | An event or series of events that disrupts the normal operation of a service. |
| **MTTR (Mean Time to Recovery)** | The average time taken to recover from an incident.                      |
| **MTTF (Mean Time to Failure)** | The average time a service or system operates before failing.             |
| **MTBF (Mean Time Between Failures)** | The average time between two consecutive failures of a system.          |
| **Postmortem**               | A detailed report and analysis after an incident to understand what went wrong and prevent future occurrences. |
| **Root Cause Analysis (RCA)** | The process of identifying the underlying cause of an incident or failure. |
| **Redundancy**               | The practice of having multiple, backup components to avoid single points of failure. |
| **Load Balancing**           | Distributing workloads across multiple resources to ensure efficient usage and prevent overloading a single resource. |
| **Chaos Engineering**        | The practice of intentionally introducing failures to test how well a system can withstand and recover from them. |
| **Scalability**              | The ability of a system to handle increased load by adding resources or optimizing performance. |
| **Capacity Planning**        | The process of determining the optimal resources (e.g., CPU, memory, storage) needed to handle a system’s workload. |
| **Resilience**               | The ability of a system to handle failures or stress without compromising its functionality or performance. |
| **Observability**            | The ability to measure, monitor, and analyze a system’s internal state using logs, metrics, and traces. |
| **Monitoring**               | The practice of continuously collecting and analyzing data from systems to ensure they are functioning as expected. |
| **Alerting**                 | The process of notifying teams about issues or anomalies detected in systems. |
| **Automation**               | The use of software tools to automatically perform repetitive tasks like deployment, testing, or recovery. |
| **DevOps**                   | A cultural and technical movement aimed at improving collaboration and automation between development and operations teams. |
| **CI/CD (Continuous Integration / Continuous Deployment)** | Practices that automate the integration of code changes and the deployment of those changes to production. |
| **Reliability**              | The probability of a system operating without failure over a given time period. |
| **Latency**                  | The time delay between a request being sent and the corresponding response being received. |
| **Throughput**               | The rate at which a system can process requests or data.                  |
| **Traffic Shaping**          | The practice of managing the amount and type of traffic sent through a system to avoid overloading it. |
| **On-Call**                  | A practice where engineers or support teams are available to respond to incidents or service disruptions. |
| **Canary Releases**          | A software release strategy where a new version is rolled out to a small subset of users to test it before a full-scale release. |
| **Blue/Green Deployment**    | A deployment method where two identical production environments (blue and green) are used, allowing for easy switching between them. |
| **Zero-Downtime Deployment** | A deployment process that ensures no downtime during the deployment of new software or infrastructure changes. |
| **Distributed System**       | A system in which components are located on different networked machines, communicating and coordinating with each other. |
| **Service Mesh**             | A dedicated infrastructure layer that handles service-to-service communication in a microservices architecture. |
| **Microservices**            | A design pattern in which an application is broken down into smaller, independent services that can be deployed and scaled independently. |
| **Auto-scaling**             | The process of automatically adjusting the number of active servers or resources in response to traffic or load. |
