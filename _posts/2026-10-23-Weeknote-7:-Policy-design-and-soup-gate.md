---
layout: post
title: "Week 7 - Shaping the Rules"
categories: weeknotes, policy design
modified_date: 2026-10-23
---

This week I became more integrated into the Transformation team. I'm picking up the signals about what matters most for our product, and getting a better sense of what we're aiming to achieve through the transformation strategy.

I also attended an NHS App design assurance gate to discuss a live, joined-up opportunity between the App and Referrals and Appointments. It was good to see how we apply the NHS service standard between Alpha and Private Beta.

### The scale of e-RS

e-RS is a big platform and, like the NHS itself, its scale is hard to grasp. <a href="https://digital.nhs.uk/services/e-referral-service" target="_blank" rel="noopener noreferrer">From January to December 2025</a> there were:

- **19.9m** total e-RS referrals (+5% year on year)
- **2.4m** Advice and Guidance (A&G) outcomes (+19% year on year)
- **1.6m** A&G outcomes returned with advice (+19% year on year)

That's around 55,000 referrals a day, or nearly 80,000 every working day.

And yet, when I visited my GP this week and asked about e-RS, she didn't know what I was talking about! Her practice uses EMIS, which integrates e-RS via the API and embeds key e-RS screens, so it feels like part of the clinical system. She also told me she doesn't manage referrals at all; that's done by the admin staff. These are the insights I need to get closer to day-to-day.

I've also finally got access to the e-RS INT (integration test) environment. I'm familiar with the journeys, but it's a relief to be able to channel my inner referrer and service provider, form my own opinions of the UI, and have deeper conversations with users.

### Re-platforming

The big question is how we approach the platform itself. There's strong confidence in the value of uplifting the technical foundations to a more responsive, modular platform, something a Google or Amazon would do without blinking. Here, we must justify the cost and show evidence that the investment is worth it.

Thankfully, we have technically fluent executive leadership. In one of our weekly Thursday evening updates, it was great to hear our exec set the expectation that "this is going to take a long time" and back the re-platforming work. Like most national digital services, the platform needs a tidy-up. All technology does as environments move to the cloud and standards evolve, but it's always harder to justify than the latest policy priority or care pathway, especially in the public sector.

### Policy design

This week we've been producing technical options papers for some of our transformation big bets. As part of this new programme, we're also thinking about how we can mobilise better to support policy and policy design.

This is one of the biggest adjustments for me coming from the private sector, where there was no direct policy equivalent. We had strategy, commercial and compliance (who set the rules), but product roles had much more autonomy to set strategy and measurable goals. Here, given the scale, impact and complexity of the work, it makes sense that we work much more closely with policymakers, the people who write the rules for the services we build. That means aligning on digital and policy intent and outcomes, user-centred test-and-learn approaches and, most importantly, language. Working in digital innovation, there can be an inherent bias that we know best. That must be parked.

We started the week with a session with Vero, a colleague from Breast Screening, a programme with quite a few similarities to e-RS. Vero recently <a href="https://lnkd.in/p/eDVquFXj" target="_blank" rel="noopener noreferrer">posted this on LinkedIn</a>:

> "Designing within existing rules can be frustrating, especially when the evidence says the rules need to change. I still remember the moment when I realised we could combine user-centred design with policy design. Instead of only designing within the rules, we can help shape them."

This is something I've wrestled with a lot since joining. In such a complex environment we need boundaries to our thinking and a focus on what we can actually change. But which boundaries are hard, and which are soft enough to change? That question has led us into a deep dive on how we could work much more closely with Policy, giving us all a better chance of driving significant change for patients and the frontline. Vero helped me identify some guiding principles for working better with Policy:

1. Start by listening
2. Trust must be earned
3. De-jargon our language
4. Align on outcomes and gather evidence
5. Create a safe space to learn together

Every day is a school day. Although I have 17 years' experience shipping digital services to consumers, I'm still learning a lot about policymaking, policy design, and how we can innovate more effectively as a blended team with policy colleagues.

### Mapping the policy landscape

We're working towards a workshop with senior policy colleagues invested in Referrals and Appointments. That means getting a strong grasp of the many relevant policy documents, their hierarchy and how they relate to one another:

**Tier 1: National strategy**

- <a href="https://www.gov.uk/government/publications/10-year-health-plan-for-england-fit-for-the-future" target="_blank" rel="noopener noreferrer">10 Year Health Plan</a> - GOV.UK

**Tier 2: Planning and mandates**

- <a href="https://www.england.nhs.uk/publication/2025-26-priorities-and-operational-planning-guidance/" target="_blank" rel="noopener noreferrer">2025/26 priorities and operational planning guidance</a> - NHS England
- <a href="https://www.england.nhs.uk/publication/medium-term-planning-framework-delivering-change-together-2026-27-to-2028-29/" target="_blank" rel="noopener noreferrer">Medium Term Planning Framework</a> - NHS England
- <a href="https://nhsproviders.org/resources/on-the-day-briefing-medium-term-planning-framework-2026-2029" target="_blank" rel="noopener noreferrer">MTPF briefing</a> - NHS Providers
- <a href="https://www.england.nhs.uk/nhs-standard-contract/26-27/consultation-documents/" target="_blank" rel="noopener noreferrer">Standard Contract 2026/27 consultation</a> - NHS England

**Tier 3: Elective and referral policy**

- <a href="https://www.england.nhs.uk/publication/reforming-elective-care-for-patients/" target="_blank" rel="noopener noreferrer">Reforming elective care for patients</a> - NHS England
- <a href="https://www.england.nhs.uk/long-read/letter-specialist-advice-elective-single-point-of-access/" target="_blank" rel="noopener noreferrer">Specialist advice and elective SPoA letter</a> - NHS England
- <a href="https://healthcareleadernews.com/news/icbs-must-develop-delivery-plan-for-advice-and-guidance/" target="_blank" rel="noopener noreferrer">A&G operational framework</a> - Healthcare Leader

**Tier 4: Operational frameworks**

- <a href="https://htn.co.uk/2025/04/03/operational-delivery-framework-for-icbs-minimum-standards-for-digital-platforms/" target="_blank" rel="noopener noreferrer">A&G framework (April 2025)</a> - HTN
- <a href="https://www.england.nhs.uk/wp-content/uploads/2025/03/prn01903-advice-and-guidance-enhanced-service-2025-26-v1.2.pdf" target="_blank" rel="noopener noreferrer">A&G Enhanced Service 2025/26 (PDF)</a> - NHS England
- <a href="https://www.england.nhs.uk/long-read/outpatient-services-a-clinical-and-operational-improvement-guide/" target="_blank" rel="noopener noreferrer">Outpatient services improvement guide</a> - NHS England

**Tier 5: Technical and implementation detail**

- <a href="https://www.llrlmc.co.uk/wp-content/uploads/2026/03/202602_Elective-SPOA_Technical-Guidance_V1.0.pdf" target="_blank" rel="noopener noreferrer">Elective SPoA technical guidance 2026/27 (PDF)</a>
- <a href="https://www.lmc.org.uk/wp-content/uploads/2026/04/GPCE-updated-guidance-on-AandG-and-SPoA.pdf" target="_blank" rel="noopener noreferrer">GPCE guidance on A&G and SPoA (PDF)</a>
- <a href="https://gettingitrightfirsttime.co.uk/tag/outpatients/" target="_blank" rel="noopener noreferrer">Outpatient resources</a> - GIRFT
- <a href="https://gettingitrightfirsttime.co.uk/girft-guide-to-standardising-follow-up-protocols-aims-to-ensure-patients-get-the-right-care-in-the-right-setting-at-the-right-time/" target="_blank" rel="noopener noreferrer">Follow-up protocols guide</a> - GIRFT
- <a href="https://digital.nhs.uk/services/e-referral-service/api/updates-and-releases/roadmap/spoa" target="_blank" rel="noopener noreferrer">e-RS API SPoA roadmap</a> - NHS England Digital
- <a href="https://www.hssib.org.uk/patient-safety-investigations/electronic-patient-record-systems-electronic-referrals-for-ongoing-care/interim-report/" target="_blank" rel="noopener noreferrer">Electronic referrals for ongoing care: interim report</a> - HSSIB
- <a href="https://westminsterbrief.co.uk/archive/pq/122778" target="_blank" rel="noopener noreferrer">PQ on patient choice</a> - Westminster Brief

We also looked at examples of good and bad policy design, and at a lot of the GDS thinking on how to do policy design better. There's a tonne of good content from Policy Lab for me to dive into (links below). Perhaps one day I'll contribute to the canon! A few examples that stood out:

- **Organ donation opt-out** changed the default, then supported families to follow it.
- **Getting It Right First Time** changed surgical practice using data and peer pressure, not rules.
- **Open Banking** used a legal order and a shared standard to open up bank data.

It's useful to start with examples like these and think about the different levers for behaviour change, and how we might deal with unintended consequences.

Next, we need to work out what this looks like as a roadmap: building out our innovation backlog and organising it by value, and by whether each item can be done in the short to medium term on the current platform or belongs in the transformation work.

## Links

- <a href="https://www.youtube.com/watch?v=OBl5rRV8OfU" target="_blank" rel="noopener noreferrer">Watch this video</a>
- <a href="https://openpolicy.blog.gov.uk/2017/09/22/designing-policy/" target="_blank" rel="noopener noreferrer">Designing policy</a> - Open Policy Making blog
- <a href="https://openpolicy.blog.gov.uk/2020/03/06/introducing-a-government-as-a-system-toolkit/" target="_blank" rel="noopener noreferrer">Introducing a government as a system toolkit</a> - Open Policy Making blog
- <a href="https://design-history.prevention-services.nhs.uk/breast-screening-pathway/2026/10/aligning-perspectives-gp-it/" target="_blank" rel="noopener noreferrer">Aligning policy, clinical and digital perspectives on communicating with GPs</a> - NHS Breast Screening design history
- <a href="https://htn.co.uk/2026/10/13/838-million-open-framework-for-future-of-gp-it-announced/" target="_blank" rel="noopener noreferrer">£838 million open framework for future of GP IT announced</a> - HTN

***

**Soup gate.** Somehow we've created a dynamic where one son likes chunky soup and the other likes smooth. Key learning: you can't unblend soup :(
