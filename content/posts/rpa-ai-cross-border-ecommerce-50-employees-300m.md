---
title: "From 170 Employees to 50: How This Cross-Border E-commerce Founder Built a 300M RMB Business with RPA and AI"
date: 2026-06-01
draft: false
tags: ["CaseStudy", "AI", "CrossBorderEcommerce", "RPA", "SmallBusiness"]
description: "Huang Xufeng scaled his cross-border e-commerce operation from 170 people to 50 while growing revenue from 100M to 300M RMB. Managing 300,000 SKUs across 187 countries, he used RPA, AI dashboards, and a self-built workflow system to achieve 10x productivity gains. Here's the exact playbook."
---

The warehouse looked nothing like what you'd expect from a 300 million RMB business. It was modest—a few rows of shelves, a handful of people clicking through browser tabs. But what those people were doing with their time told a different story.

"Before RPA, we had 170 people doing maybe 100 million RMB in revenue," Huang Xufeng told me over a video call from his office in Shenzhen. "Today we have 50 people and we're doing 300 million."

The math doesn't add up the way most people assume. This isn't a story about replacing humans with machines. It's about understanding which human work is fundamentally automatable, and which requires actual judgment—and then building the infrastructure to separate the two.

Huang is a 14-year veteran of cross-border e-commerce. He sells on platforms across 187 countries, manages roughly 300,000 SKUs (of which about 30% have ever generated an order), and has been through enough cycles to know what "scaling wrong" looks like. "I lost tens of millions in my early years," he said. "Because I was young and felt invincible. I wanted to try everything."

The transformation began in April 2023, when he saw a video about RPA—a robotic process automation tool—and bought a license the next day.

## The Three Bottlenecks That Were Killing the Business

Huang breaks down the cross-border e-commerce operation into three layers that every player in this space has to manage: sourcing and selection, content creation and listing, and publishing and post-sale. Each layer was its own kind of hell.

**Bottleneck 1: Human Error and Scalability**

The first problem was foundational. Every platform listing required 60+ steps—screenshots, field entries, pricing calculations, logistics estimates. Huang had built a 500-page SOP document to train new employees. Three hours of self-study and they could do the job.

Except they couldn't. Not really.

"The SOP solved training speed, but not accuracy," Huang explained. "If someone mistyped a price, that was a direct loss. We carried those losses. Management overhead was enormous."

The math: when you have 200 stores, each potentially carrying 3,000 products, you're managing hundreds of thousands of SKUs. At 1,000 new listings per day, the operational risk was constant and compounding.

**Bottleneck 2: High Employee Turnover**

The work was basic. Input numbers, click buttons, follow steps. It required no judgment, no creativity, and offered no growth path. Employees came, learned, and left—sometimes within weeks. Every departure meant lost institutional knowledge and renewed training costs.

"Ideally you'd want people who could grow into the system," Huang said. "But the work didn't allow it. The machines weren't ready yet to take over, and the humans were stuck doing machine work."

**Bottleneck 3: Decision Lag**

When revenue spans 187 countries and hundreds of thousands of products, data is supposed to be king. But the data was slow. Really slow.

"Before our AI dashboard, if the boss wanted to know why a store's numbers dropped today, the process was: check ERP, find the order anomaly, forward to the relevant manager, manager investigates, traces back to the specific listing, identifies the cause, reports back up the chain."

Huang pauses. "Minimum decision cycle: four to six hours. And if the boss was busy that day, maybe it got addressed tomorrow. Or never."

## The Solution: A Three-Layer Automation Stack

Huang's fix wasn't a single product or a single "AI solution." It was a layered approach, each layer solving a different problem.

**Layer 1: RPA for Repetitive Operations**

The breakthrough came from an unlikely place—a video his junior college classmate sent him about a Japanese RPA tool called UiPath. Huang bought it the next day and started figuring it out himself.

But here's the part most companies miss: he didn't just hire an RPA specialist to handle it all. Instead, he had all 160 employees get UiPath certified.

"We made everyone study," Huang said. "Watch videos, practice, take the exam. About 9 hours per person. Because I realized the bottleneck wasn't the RPA developer. It was that non-IT people couldn't articulate what they needed."

By certifying everyone, he suddenly had 100+ people who understood the boundaries and possibilities of the tool. "Suddenly we had 100 people who could actually submit useful automation requests. That's when it started working."

The first wave: automate the repetitive, standardized tasks that had high turnover and high error rates. One click, automated operation, running 24 hours. The effect was immediate—operational load dropped significantly.

**Layer 2: AI Dashboard for Real-Time Decisions**

The second layer addressed the decision lag. Huang used RPA to pull data from all platforms into a multi-dimensional spreadsheet, then built a BI dashboard that automatically pushed alerts to WeChat.

Not "you can check the dashboard." Push. "Look at this listing. Its conversion rate dropped. Fix it."

The difference between a traditional BI system and an AI push system is subtle but massive: passive monitoring vs. mandatory notification. "Before, you had to actively look. Now, whether you look or not, it comes to you. You have to see it."

Decision cycle: from 4-6 hours to real-time.

**Layer 3: Workflow Systems for Product Generation**

The third layer was more sophisticated. Huang built an internal web interface that accepts a single product photo and generates a complete set of listing images—white background, size charts, model shots, detail shots.

"Different stages of the workflow use different models," he explained. "Some stages are just background removal—any free tool handles that. Other stages need more complex generation. In total, maybe 2-3 RMB per finished set of images."

The whole system is integrated with Feishu (Lark), so staff can upload a photo and get back a complete set of ready-to-use product images within minutes.

## The Org Structure Change: From Vertical to Horizontal

The automation didn't just change the work—it changed the organizational structure.

"Before, we had about 20 platforms, each with its own leader vertically downward. If I wanted to launch 20 platforms, I'd need 20 leaders."

Now: one leader per function (selection, publishing, post-sale) across all platforms. Because RPA handles platform-specific execution, and AI dashboards handle cross-platform visibility, you don't need a dedicated human for each platform anymore.

"Within our system, we only need to抓住几个点—the key leverage points. Selection is still human. After that, once the product is chosen and assigned to a platform, the rest is automated."

## The IT Team Structure

Huang's internal IT organization is unusual. It has three tracks:

**1. RPA BP (Business Partner)**: Ensures program stability across operations.

**2. Database**: Breaks down data silos, connects platform APIs, builds the company's own data infrastructure.

**3. AI Selection Officer**: A dedicated role—budget capped at 20,000 RMB/month—whose job is to research emerging AI tools, test them, and produce structured reports on what works and what doesn't.

"This person tells me: 'Here's what Kling can do. Here's what Sora can do. Here's what it would cost to implement. Here's the expected efficiency gain.'" Huang calls this "preventing blind trial and error."

## The 85% Mark and What's Left

I asked Huang where he currently stands on the automation journey. "About 85%," he said. "The remaining 15% is both a technical壁垒 and a mental one."

His 2026 goal is 95%. The path forward is using tools like the recently released OpenAI Operator or similar enterprise-grade agents to build what he calls "an OS for cross-border e-commerce."

"Imagine a single interface where I tell the AI: 'I want to launch this product on 5 platforms, with these margin targets, and I want a report in 3 months.' The system then autonomously handles: sourcing data, publishing listings, forming ad campaigns, tracking performance, and reporting back. If it misses the target, it self-corrects."

The catch: this requires the company's own foundational systems to be AI-ready first. Data cleaned. SOPs documented. Knowledge bases built.

"You can't just throw an agent at a messy operation and expect magic," Huang said. "That's the mistake many small businesses make. They want the big lobster to fall from the sky and solve everything at once. It doesn't work that way."

## The Mistakes Small Businesses Make with AI

When I asked about common pitfalls, Huang was direct: "Most business owners don't want to start from the smallest thing. They want a big solution from day one. But AI adoption doesn't work that way."

His prescription: find the single most painful point in your operation, document it as an SOP, then use RPA to automate that one thing. "Find the feeling of breakthrough. Once you see the computer working for you, you'll understand. The feeling is incomparable."

He's also honest about his own mistakes: "I once lost tens of millions because I tried to do everything at once. Don't copy that. Find one pain point, solve it completely, then move to the next."

## The Future: Humans in the Right Roles

The deeper question Huang grapples with isn't automation—it's what humans should actually be doing.

"Every job in our company has multiple people, and each person thinks they're the most important," he said with a laugh. "But when I rotated through every role myself, testing each person's methods, I found something interesting: which listing you write doesn't actually determine if a product sells. The product itself, the keyword selection, whether the market is hot right now—these matter more."

So what should humans do? "Judgment. Strategy. The things that require taste, instinct, and business sense." Everything else? "Let the machines handle it."

## FAQ

**Q: How did Huang Xufeng grow from 170 employees to 50 while increasing revenue from 100M to 300M RMB?**

A: By implementing a three-layer automation stack: 1) RPA for repetitive operational tasks (listing uploads, data entry), 2) AI-powered push dashboards for real-time decision-making, and 3) internal workflow tools that auto-generate product images and listing content. This allowed the company to operate with fewer people managing far more products and platforms, scaling revenue 3x while cutting headcount by 70%.

**Q: What is the single biggest mistake small businesses make when adopting AI?**

A: They want a magical "one-click" solution from day one, without doing the foundational work first. Huang's key advice: start with the single most painful process in your business, document it as an SOP, then use RPA to automate just that one thing. Find the feeling of breakthrough before trying to scale. Without clean data, documented processes, and AI-ready infrastructure, even the most powerful agent will fail.

**Q: How does the cross-border e-commerce selection process actually work?**

A: The company uses a data-driven approach: for each potential product, they calculate cost from 1688 + international logistics + platform fees, then determine if profit margin targets are met. Products are assigned to specific platforms (Amazon, AliExpress, etc.) based on financial核算体系和回款周期. One person can select about 150 products per day. Listings are then auto-published via RPA, with ongoing monitoring via AI dashboard.

**Q: What is the AI Selection Officer role in Huang's company?**

A: A dedicated role with a 20,000 RMB/month budget to research, test, and evaluate new AI tools and models. The person produces structured reports on which tools can improve efficiency, at what cost, and with what expected ROI. This prevents the company from making blind investments in trendy but ineffective AI products. Huang believes this is critical because "going out and learning" has become a prerequisite for staying relevant in cross-border e-commerce.

**Q: Why did Huang make all 160 employees get UiPath RPA certified?**

A: Because RPA implementation fails in most companies not because of technical limitations, but because non-IT employees can't articulate what they need. By certifying everyone, everyone understood the boundaries and possibilities of the tool. Suddenly 100+ people could submit useful automation requests instead of one IT person trying to extract needs from people who couldn't explain them. This "全员考证" (everyone gets certified) approach transformed how the company identified automation opportunities.