# Coupling Field Guide

*Where coupling comes from, and what you can do about it*

For any coupling you meet: where does it come from, who can change it, and what does it cost?

## Front

### Three terms

- **Origin:** where a coupling comes from. Always a decision, made deliberately or by default.
- **Symptom:** where it hurts.
- **Dimensions:** Khononov's three: integration strength, distance, volatility.

### Khononov in 30 seconds [Khononov 2025]

- **Integration strength:** what one side must know about the other. From strong to weak: *intrusive* (private internals), *functional* (business logic), *model* (the internal model), *contract* (an interface built for integration).
- **Distance:** how far apart the coupled parts are: same method, module, service, team, company. Ownership counts as distance.
- **Volatility:** how often the upstream side changes. Core subdomains change often, supporting and generic ones rarely.

`BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY`

| Strength \ Distance | close | far |
|---|---|---|
| **strong** | **Cohesion**: good coupling *(balanced)* | **Global complexity** when it changes. **Stable coupling** when it rarely changes *(balanced)* |
| **weak** | **Local complexity**: unrelated things side by side | **Loose coupling** *(balanced)* |

**Stable coupling,** e.g. writing straight into a bought product's database that hardly changes.

- **Balance does not last:** new features add strength, splits and reorganisations add distance, a new business strategy changes volatility.
- The same rules hold from methods to services.

### The four origins

| Origin | What couples here | Who names it | Typical owners |
|---|---|---|---|
| **Domain**: the business, including its processes | shared business concepts and rules, process handovers, regulation, a resource the business has only once | **DDD**, Team Topologies, Khononov | the business (process owners, product management); legislators, regulators and standards bodies when the rule is given |
| **Organisation**: the development organisation (teams, roles, process, lifecycle) | ownership, team boundaries, handoffs, joint releases, central teams and platforms | **DDD**, **Team Topologies**, Khononov, Conway, change coupling | management (team structure, mandates, budgets); platform teams for their platform; upstream teams for what downstream teams must follow; architects, who design how teams interact |
| **Design**: boundaries, contracts, integration | what crosses a boundary, how far, how often it changes; a database or infrastructure shared for integration | **DDD**, Team Topologies, **Khononov**, Conway, change coupling, **Hohpe's facets**, (a\|e)fferent, connascence | the architect or architecture team; an architecture board or enterprise architecture that sets constraints; whoever chose a framework or product |
| **Code**: the implementation | names, meanings, call order, shared libraries and global state, duplicated rules | DDD, **Khononov**, **change coupling**, Hohpe's facets, (a\|e)fferent, **connascence**, **six levels\*** | the team that owns the component (exactly one per part); for shared code its maintainers, often a platform team; for third-party code its vendor or community |

*Who names it* is a bubble map on the card; here as text, **bold** = strongest (darker on the card).

**Reading the map:** a bubble covers the origins a vocabulary names; darker where it is strongest. Khononov reaches the organisation through distance (ownership counts). DDD's context maps say how teams relate. Change coupling from the version history shows the effect, never the cause.

\* Six levels: Stevens, Myers, Constantine, Yourdon [Stevens+1974], [Myers 1975], [Yourdon+1978]

## Back

### Trace and decide: which coupling do you live with, and which do you remove?

1. **What do we observe?** Start from a symptom:

   *Code and history*

   - A change in one module forces changes in others.
   - Files keep changing together in the version history.
   - You change code for no functional reason, because unrelated things sit together.
   - You struggle to find the place you have to change.
   - A change breaks something that seemed unrelated.

   *Delivery*

   - Separately built services are tested together and released as one set.
   - One build rebuilds everything.
   - Several applications share one database schema, and every change needs one team's approval.

   *Runtime*

   - A service fails or slows down because a service it calls does.
   - Services act on stale or inconsistent data.

   *Organisation*

   - Teams wait for a central team (testing, operations, database) before they can ship.
   - A downstream team is at the mercy of upstream priorities. The upstream team is afraid to break downstream systems.
   - Teams that should coordinate don't: more defects, slower work.
   - Several teams change the same component, and nobody owns the result.
   - Every design decision waits for one architect.

   *Business*

   - Every feature takes longer and costs more than the one before.
   - The business stops evolving a feature because the code is too tangled to change.

2. **Which decisions created it, or keep it alive?** Ask in all four origins (front). Expect more than one.
3. **Who owns each decision?** Who should decide it, and who decides it in fact? When the two differ, the decision has drifted. Into the system: the business stops evolving a feature because the code won't let it [Khononov 2025]. Or to another unit: business decisions trickle down to technical teams [Skelton+2025]. The lasting fix lies with the owner who should decide.
4. **What can we do, and what does it cost?**

   - *Weaken what crosses the boundary:* an interface built for integration instead of your internal model; public integration events instead of your internal ones; an anti-corruption layer; one owner for duplicated logic.
   - *Shorten the distance:* move what changes together into one module or one team.
   - *Isolate volatility:* put the part that changes often behind a stable interface; split along change cadence.
   - *Change the decision outside the code:* the business clarifies the rule or the process; management changes team boundaries or how teams interact; somebody takes ownership.
   - *Absorb it:* contract tests, release coordination, an extra check where it hurts. These costs never stop.
   - *Accept and record it:* decision record or risk list, with a trigger to revisit.

   For each option, estimate the cost **now** (build it, risk of the change), **ongoing** (grows with volatility) and **later** (lock-in, exit cost, options lost).

### Who can move the decision?

This sets your options for dealing with the coupling.

| The owner is … | Your options |
|---|---|
| reachable and willing | Change the decision where it was made. |
| reachable, but won't move (reasons may stay undisclosed) | Treat it as a constraint and contain it. Write down the consequences (cost, risk) and take them to the owner. No channel for that? Create one: a risk list they review, a decision record they sign off. |
| external: vendor, open-source project, other company | Negotiate, pay, contribute or switch. Meanwhile isolate it and keep switching possible: your own interface in front, your data exportable, no extras you can't replace. |
| out of reach: law, regulator, standard | Treat it as given. Contain it and keep it close: one module, one team, so a change stays local. |
| nobody | Name an owner first. |

**Live with it** when the decision can't move, or keeping it costs less than changing it. Keep it close and record it: decision, owner, cost, revisit trigger.

**Remove it** where it was decided, if you can influence that decision. Otherwise contain it in the nearest place you control.

---

*Coupling Field Guide v1.0 · companion to the talk “Where Coupling Comes From”, Software Architecture Gathering 2026*  
*Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) · sources: see README*  
*Alexander Lorz - alexander@lorz-it.de 11/2026*  
*https://github.com/alxlo/coupling-field-guide*
