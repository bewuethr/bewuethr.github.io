---
toc:true
---

# StaffPlus New York 2022

## Silvia: Being a Principal Engineer

- Senior IC means making technical decisions and leading, i.e., not do
  everything yourself
- More effective if you report to a senior leader and partner with managers
- Input on teams' performance
- Product strategy to technical strategy to technical execution
- Identifying one way doors
- Don't micro plan, steer the ship to the Bay of appropriate futures, plus a
  lighthouse
- Not being an architecture astronaut: be involved in incidents
- Keep contacts outside of engineering

## Ryan: Focusing on the most important thing

- Bad: inertia, decibels, guilt
- Ritual: empty inbox, reflect on last week, choose your work
- Identify bad patterns: delegate? What should I be working on? What feels
  urgent, who cares if I don't do it? Would the CFO think I'm irreplaceable? Am
  I better at the end of the week than the beginning?

## Amy: Starting a new job as Staff+

- Why were you hired? And why externally? Explore the need, identify the
  problem you were hired to solve
- Build your network: understand organizational context; focus on relations
  that violate Conway's law
- What's your job? Assume nothing, set expectations? Clarify archetypes. How do
  you define staff, how do you know somebody is ready for it? What's top of
  your manager's mind?
- What (wrong) lessons did you learn? Don't over index on past experience;
  example: acquisition doesn't have to kill innovation
- What do you tackle first? agree on a timeline
- What can you uniquely do as a new person? Re-examine assumptions, capture
  knowledge, be kind, tackle paper cuts
- When do you know enough? Learn through experimentation, or doing? Is there a
  mismatch between your position on the spectrum and the company's? Is the
  mismatch why you were hired? Breadth: Draw infrastructure and architecture
  diagrams; map out a request path; depth: most complex piece of code? Where
  are the logs?
- Eventually, forget these questions

## Michelle: API design lifecycle

1. Start with quickstart guide
2. Model the domain; don't shy away from non technical complexity
   - Learn the domain: integrate, read docs
   - Design the model
   - Test against use cases
3. Package the model: simplify
4. Test and iterate: build lots of integrations, remove friction

## Katie: Long term projects

- Example project: turning Etsy into a PWA
- Rebuild or refactor? Decided to refactor, but that's slower to market
- To get to Mars, you have to get to the moon first

1. Partner with teams outside your org; highlight benefits to, e.g., business;
   look at where the org lines meet and then "cross the A" (network, lurk in
   Slack channels, build a reputation for good customer service)
2. Investing in infra today enables future work; infra work can be valuable in
   itself; show that for every step (break down work that each step provides
   value on its own)
3. Get feedback early and often: derisk early stages, test each stage; measure
   continuously, start with measurable goal, define success, measure and track
   over time; if non-shippable: simulate, send out rovers, run temporary
   experiments
4. Architect as a force multiplier: architect for technical direction, tech
   lead to break work down; role can change over time; level up your team
   (handle ambiguity, changes in plans, trade-offs, data driven decisions)

## Mattie: Getting to commitment

1. Know your goal: convince yourself, what don't others know, what will change
2. Focus on values: deeper meaning; changing org values is very hard, use
   existing alignment, reframe goals to better align
3. Address your audience: understand who they are, what they need; more than
   formal approver, differentiate between goals and audience needs, proxies to
   expand reach
4. Present progress: can define success; responsible and honest communication,
   opportunity to focus
5. Open your mind: don't be the bottleneck, build context in others, help
   others make your idea their own

## Bobby: Inefficiency challenges at datacenter scale

- SRE: avoid operational burden; don't get paged because you didn't plan for
  capacity
- Datacenter scale: Twitter runs their own datacenters and builds their own
  computers
- Default node (standard hardware)
- Compute and storage
- Compute runs on Aurora and Mesos; CPU most important
- Storage: key value store, blob store; CPU least important
- Idea: better utilize CPU in storage
- Leveraged cgroups and namespaces
- Combine storage and compute in a single node, cut hardware cost in half,
  created 10% capacity for "free"

## Leslie: Influencing

- Influencing without direct reports and budget
- Learning from social media influencers
- Name recognition: can't influence if nobody knows who you are; start by
  bragging about achievements
- Personal brand: things you like (cat lady), clothing, what you do
- People should want to work with you: build trust; finish things (requires
  focusing)
- Know when to say no, set realistic expectations regarding your involvement;
  point to other people who can help
- Listen: you'll better understand how to help if you give people space; be an
  idea aggregator
- Building a network: share your influence, be a sponsor/mentor; learn to
  delegate
- Work to put yourself out of a job
- Diversification: be involved in as many projects as you can (but what about
  saying no?)
- Get outside the office: give back to the community, volunteer, it might take
  you places!

<https://leaddev.com/leaddev-live/delegation-equation>

## Alice: Accessibility

- Solve for one, extend to many (ramps: wheelchairs, but also strollers)
- Screen readers: remember ancient HTML that was already semantic; CSS
  introduced "divitis", and only later semantic HTML became popular
- Landmark navigation: give context for screenreaders
- Language attributes as hint for screenreaders
- Light mode on screens causes digital eye strain; dark mode is easier on the
  eyes and improves legibility; but respect user preference

## Scott: Ambiguous projects

- Projects where you can't even see the proper outline, unknown tech, new team
  dynamics
- Also an opportunity: you can make it what you want it to be
- The closer to the level "mission statement" you get, the less clear things
  are
- Before project: ambiguous goals (customers unimpressed, poorly targeted) -
  write a product brief (PRFAQ); provide specificity and clarity
- Who needs to be involved? Be specific (e.g., RACI), have stakeholders review
  product brief; tight group of visionaries (heist team assembly montage)
- Define intent early and iterate; provide a "known mediocre" proposal instead
  of trying to be perfect
- If you get lost talking about the project, workshop the right words and be
  very consistent
- Execution: accelerate out of the unknown; keep an eye out toward surprises;
  socialize and get feedback; log and broadcast decisions in public (articles:
  "technical decision-making and alignment in a remote culture", "don't ask
  forgiveness, radiate intent"); produce frequent incremental value
- Exit criteria: pick a finish line and repeat it a lot
- Managing yourself: you can protect the team from uncertainty; radiate
  resilience and calm when you hear bad news; bend and adjust with pressure
- Exploring the frontier of what you're comfortable with can lead to tremendous
  growth

<https://medium.com/@ElizAyer/dont-ask-forgiveness-radiate-intent-d36fd22393a3>
<https://multithreaded.stitchfix.com/blog/2020/12/07/remote-decision-making/>
<https://leaddev.com/agile-other-ways-working/landing-projects-successfully>
<https://leaddev.com/technical-decision-making/strategies-making-impossible-decisions>

## John: Decision buy-in

- Decision: most senior engineer? Everybody votes? Wisdom of the crowds?
  Multiple independent judgments > individual expert judgment
- Watch out for anchoring; form opinions before knowing other opinions;
  interpret disagreements correctly, pre-commit to decision strategy, eliminate
  emotions
- Saaty: analytic hierarchy process (AHP)
- Goal: choosing the most suitable leader
- Criteria: experience, etc.
- Alternatives: candidates
- Start with pairwise comparisons per criterion, then compare criteria
- Table per criterion with reciprocal scoring, gives you a priority vector, get
  weighted values
- Source weights by survey or story point like discussion, which could be more
  valuable than the tool outcome itself
- AHP retrospective: used successfully by multiple teams; graphs can be used in
  ADRs; trying to hide the data from the process doesn't work because of
  nemawashi (building consensus openly, and not forcing consensus openly)

## Bryan: Building bridges

- There is no road, it's all swamp
- There is no how-to
- Leading without being the boss: be a good example
- It's about scope: you go up, scope increases
- Influence: if you 2x five people, you're a 10x engineer
- Learn to delegate: search for levels of delegation online
- Be a bridge: connect things that don't want to be together
- Failure and shame: build a culture that is tolerant of failure
- Input vs. outputs: you can't control the outputs, so don't think about them;
  you can't will the outcomes
- Golden circle: why, how, what; we tend to focus on the what and forget to ask
  about the why
- Write things down
- Embrace uncertainty
- Don't stop growing

<https://simonsinek.com/product/start-with-why/>

## David: Building peer groups

- Examples: new hire peer groups, peer group for new engineering managers
- Staff engineer community: low barrier of entry (Slack channel, Google group,
  zoom calls twice a month, occasional dinner)
- Mostly talking to each other, bring up issues, identify common problems
- New: start tech talk series, mentorship program; not limited to, but run by
  Staff+
- Get the scope right; 50 people total, 10 in a zoom call
- Self-organize the group
- Hire people who care about community
- Group should provide value for its members; for example, having a sympathetic
  ear

<https://www.daviddaly.me/p/peer-group-resources.html>
