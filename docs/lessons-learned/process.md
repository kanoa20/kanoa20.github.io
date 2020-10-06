---
layout: default
title: Zero-to-One Process
parent: Lessons Learned
nav_order: 3
---



# Zero-to-One Overview: From concept to launch

---
{: .toc }

<details close markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
* TOC
{:toc}
</details>

---

This section describes some of the important lessons learned I've gathered along the way as a product leader that I think are the keys to having a successful zero-to-one launch. Essentially, there are five phases:
1. **Concept:** Use customer feedback on proof of concepts to build a prioritized backlog of use cases
2. **Define:** Create a measurable strategy that aligns stakeholder success metrics with minimum desired product for customers
3. **Design:** Map use cases to building blocks to identify reusable components, design limitations and skillset needs
4. **Build:** Work backwards from release milestones, projected resources and key dependencies to identify scope (use cases) that can be completed
5. **Launch:** Align go-to-market plans with target customer segments for that release and always have a launch checklist for go/no-go.

In the following sections, we'll go through a simplified concept to launch for a guest experience management service.

## 0. What is guest experience management?
A guest experience management service provides a consolidated way for hotels to improve a guest's experience at every touchpoint of their stay. Below is a simplified backlog of seven use cases for guest experience management. We will use this list as a baseline for what we plan to cover in the sections below.

_Figure 1: Simplified backlog of use cases for guest experience management services for hotels_

![](/assets/images/0_to_1.jpg)


## 1. Fail fast with proof of concepts
During the **concept** phase, our primary goal is to understand our value proposition across a diverse customer base to determine our serviceable market:

1. Interview customers about their current problems (use cases) to create a request backlog
2. Build and test hypotheses with demo solutions to get a good sense of priority for each use case
3. Be juidicious with customer time and reward them for their commitment

In the simplified guest experience example, our value proposition is to improve guest satisfaction while reducing hotel operating costs. If we take a look at Figure 2, we tested two concepts based on pain points mentioned by customers:

* **Skip the front desk:** Addressing pain points for guests who want a fully automated checkin and checkout experience
* **Service optimization:** Addressing pain points to provide guests better service during their stay.

Overall, skip the front desk use cases had a higher value proposition than service optimzation from hotel feedback.

_Figure 2: Themes derived from proof of concepts and value proposition derived from scoring customer feedback_

![](/assets/images/0_to_1_step_0.jpg)

---
## 2. Get stakeholders aligned on measuring success
During the **define** phase, our primary goal is to define our business and product strategy:

1. Educate stakeholders on the customer problems, segments and market opportunity
2. Align the stages of customer engagement with stakeholder expectations of success
3. Create a business model that quantifies expected, conservative and aggressive projections.

In the simplified guest experience example, we are able to define two hotel segments (luxury and boutiques). The primary difference between hotel segments is the type of guests that frequent these hotels which dictates the services these hotels provide. Luxury hotels catered primarily to business travelers that required room service. These guests especially preferred privacy so extra precaution was taken for housekeeping and maintenance visits by adding infrared sensors in the guest rooms to know when rooms were vacant for hotel staff to service. Guests that visited boutique hotels were primarily millennials who were more likely to explore the city and were less likely to use these extra features. Since use case 1-3 are high value proposition needed by both hotel segments, we can align stakeholders to hotel adoption metrics that measure the growth in number of hotel rooms. Use case 4-5 were focused on service optimization and had less of an overall value proposition; however, we felt that the frequency of engagement would be much higher for certain guests and so we expected to measure higher usage for certain cohorts of hotels and guests. Finally, step #2 will highlight in more detail why keyless (BLE) locks and in-room (telemetry) sensors align with a reduction in acquisition costs.

We also looked at two substitutes (HotSOS, Monscierge). Amadeus HotSOS is a leader in the service optimization space; while Monscierge is a startup providing guest experience services. Based on a comparison to these services, if we planned to support all seven use cases, we could position our product at a higher value than existing substitutes. Assuming these two subsitutes were the only major competitors in the space, we could also model our business with reasonably high adoption relative to the serviceable market.

_Figure 3: Using value proposition, customer segmentation and substitutes to align stakeholder success metrics_

![](/assets/images/0_to_1_step_1.jpg)

---
## 3. Identify design limitations early on
During the **design** phase, our primary goal is to architect our solution:

1. Breakdown use cases into user stories that highlight functional and non-functional requirements
2. Derive flow diagrams and key components (building blocks) needed to support these user stories
3. Identify make, buy, lease trade-offs for each component and skillsets required to execute

In the guest experience example, we can assume that use cases map 1-to-1 with user stories for simplicity. We can then build a value chain map that assesses the maturity and visibility of a component for guest and staff end users. For example, we can see in Figure 4 that guests would interface with a mobile app to checkin remotely (use case #1). When a guest checks-in through their mobile app, a message is sent to the single sign-on service via the cloud gateway to authenticate the user. Once the user is authenticated, the checkin payload is sent via the message broker to our reservation state machine running on a compute component that processes check-in business logic and stores the guest checkin. Once this is validated, the guest receives a checkin confirmation on their mobile app.

Once we create all the major components of our guest experience example, we can evaluate whether to 1/ lease (cloud), 2/ build (applications), and 3/ partner (device integrations) for each component. For partnering, we can see that there are high areas of inertia that present opportunities, risks and challenges. This inertia presents an opportunity for guest experience services to create value by using the cloud to bridge the gap. In this case, since devices like locks, sensors and edge gateways that connect to the cloud are less mature, it makes sense to partner with providers who are already working on device-side solutions in this space rather than try to build our own. If we revisit Figure 3, the initial partnerships with locks, sensor and gateway companies would increase acquisition costs initially as we build out our initial custom prototypes, betas and first deployments. However, if we prioritize the right partners that have a large enough network effect, we can collectively start to scale our adoption and usage in hotels while reducing our overall acquisition cost.

As mentioned above, this inertia also creates risks and challenges. Many devices speak a different messaging protocol than the cloud which requires translation at the edge. Additionally, there are added latency required to send messages from a device-to-cloud-to-mobile (and back). Finally, most edge devices work behind network firewalls and communication is often unecrypted which creates additional development work needed to encrypt messages before communicating with the cloud. These risks and challenges can present frictions to scale if left unaddressed.


_Figure 4: Value chain map identifying make, buy, lease trade-off_

![](/assets/images/0_to_1_step_2_1.jpg)

During this **design** phase, we can also use these value chain maps similarly to organizationally structure our teams and understand skillsets that we would need in order to support this development (figure 5). In the guest experience business, it would be beneficial to have 2 teams focused on guest and staff application experience, a devices team focused on partnerships and integrations, and a cloud team focused on configuring cloud components in a scalable, secure way.

_Figure 5: Value chain map identifying skillset needs_

![](/assets/images/0_to_1_step_2_2.jpg)

_[Source: Methodology derived from Wardley mapping](https://blog.gardeviance.org/)_

---

## 4. Establish demonstrable milestones for delivery

During the **build** phase, our primary goal is to plan and execute:

1. Create and update tactical roadmap with quarterly milestone delivery dates and project plans (e.g. scope, resources, dependencies)
2. Establish a "show and tell" release cadence (sprint) where teams can showcase new features they've deployed bi-weekly or monthly
3. Maintain a RAID log for weekly stakeholder review where teams can surface risks, assumptions, issues and dependencies that impact deliverables

In our simplified guest experience example, we create a tactical roadmap with three release milestones. The first release prioritizes the highest value proposition use cases (#1-3) that provide core features of a guest experience service for skip the front desk, which was identified as a major thematic problem for hotels. From the value chain maps, we would need cloud, enterprise and mobile teams; however, we would focus on leasing (and reusing) as many cloud service components and solutions as possible while building out our custom enterprise and mobile applications. For our second release, we plan to continue focusing on use cases (#4-5) that complete our skip the front desk experience while addressing the highest value proposition for service optimization themes. These use cases should drive higher value for customers which should increase likelihood for retention of customers that have already adopted. The third release addresses the remainder of our service optimization theme. Within each of these releases, we can set up monthly demos for each of these experiences and weekly RAID log reviews to ensure cross-functional teams can surface any issues.

_Figure 6: Using thematic problems, value proposition and cross-functional teams as inputs to release milestones_

![](/assets/images/0_to_1_step_3.jpg)

---

## 5. Let lighthouse customers lead when going to market

During the **launch** phase, our primary goal is to get customers up-and-running:

1. Work with lighthouse customers in each segment to understand engagement funnel steps (or channels) for both decision-makers and end users to create messaging, merchandising and call-to-actions for each channel that align with the discovery process of other customers
2. Create a GTM plan to re-engage the top 50 customers in the sales pipeline for each target segment for each release
3. Add feedback and analytics tracking mechanisms that make it easy to work with customers to reduce their onboarding and usability frictions

In our simplified guest experience example, we can see that based on feedback from lighthouse customers, the luxury segment expects engagement through website, support, emails, press releases and video channels; whereas boutique segment does not expect their teams and decision-makers to engage with videos. We could go into more detail on how to structure the content for each channel or sequence of channels in each segment's engagement funnel, but for now, these topics are outside the scope of this write-up. By understanding the funnel for each segment, we can also make it easy for self-service customers to discover, learn and adopt our services with minimal acquisition cost, while we re-engage some of the other major hotel operators in the luxury and boutique segments.

_Figure 7: Understanding the engagement funnel for each customer segment identifies what channels should be prioritized for GTM for each release_

![](/assets/images/0_to_1_step_4.jpg)

---

## Bonus: Add balance to your launch checklist
Some final thoughts on execution...

During the **build** and **launch** phase, we should always plan balance in our sprints and release milestones to prevent technical and operational debt from compounding into an insurmountible issue in the future. Remember...launching a feature is the starting line, not the finish line.

1. For any release (major, minor, patch), we should _always_ have a go/no-go checklist that includes integration and load tests, security reviews and rollback plans
2. For major and minor releases, we should plan to refactor tech debt (e.g. monolithic code) and augment operational processes (e.g. deployment pipelines) to preserve the sanity of our future selves
3. For major release cycles, we should plan to release several small iterations to internal or customer-facing beta programs that can help collect early feedback that can inform future roadmap decisions

I'd like to leave you with my guidelines for managing a sprint. I expect only 25% of the work completed in a sprint to be actual design and development. This is due to the fact that an experienced engineering manager will usually estimate ~2x the effort to deliver a feature. On top of that, my estimate for each sprint is 50% for the feature, 25% for bug bashes and user-level testing, and 25% for support, tech debt and process improvements.


---


