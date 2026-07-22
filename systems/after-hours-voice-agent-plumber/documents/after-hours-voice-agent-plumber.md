<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# After-Hours Voice Agent for a Plumber

**Project Link:** [View Project](https://nextwork.ai/projects/f95effcd-cd0e-42e5-bd4f-b37d9abe598c)

**Author:** Roy Piring Jr  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_oz2v2nu9)

## The Business Case: Auditing a Plumber's Revenue Leak

### Project vision and the advisory value proposition

In this build, I created an after-hours voice agent concept for a plumbing business. The system was designed to catch missed calls, sort emergencies from routine jobs, and connect the right call path to booking, paging, or callback.

The advisory layer was the real value driver. Before building the agent, I audited call logs, modeled the missed revenue, and defined a triage taxonomy so the agent would solve a measured business problem instead of acting like a generic chatbot.

This mattered because missed after-hours plumbing calls can include both revenue loss and safety risk. The system had to protect the business from lost jobs while also making sure emergencies were escalated instead of pushed into a routine booking path.

### Turning missed calls into a measurable dollar figure

I loaded the plumber’s freeze-weekend call log and used the analysis script to model the revenue leak from missed calls. The model separated emergency misses from routine misses, then applied the assumed ticket values and booking rates to estimate the missed opportunity.

The quarterly model came to $163,440. It used 12 emergency misses at $2,100 each, plus 10 routine misses at $340 each with a 0.6 booking factor, then multiplied that result by 6 assumed freeze weekends per quarter.

That number stayed labeled as an estimate, not a guarantee. Only $4,200 was directly measured through two callers who confirmed hiring a competitor. The rest depended on assumptions about unrecorded outcomes, freeze-weekend frequency, emergency booking behavior, routine booking conversion, and average ticket values.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_m8kdbh9b)

### Why the quarterly leak model is an estimate, not a guarantee

The model calculated the leak as 12 emergency misses times $2,100, plus 10 routine misses times $340 times 0.6, then multiplied the result by 6 assumed freeze weekends per quarter. That produced a quarterly estimate of $163,440.

It was an estimate because only $4,200 was directly measured. That measured amount came from two callers who confirmed they hired a competitor.

The rest was inference. The model depended on 6 freeze weekends per quarter, emergency misses booking at an implicit 100%, routine misses booking at 60%, and one average ticket price applied to 20 unpriced calls. The report stated the first assumption clearly, but the other assumptions still carried the estimate.

## Designing the Triage Rules and Attribution Framework

### Decisions that govern every call before code is written

In this step, I wrote the decision layer that governed the voice agent before the call flow was built. The decision document covered platform choice, triage rules, disclosure, no-quoting behavior, and attribution.

I also defined the triage taxonomy with four emergency categories and their required safety instructions. That taxonomy gave the agent a clear rule set for gas leaks, flooding, electrical risk, and no-heat situations.

The validation script checked the taxonomy against the required schema. That mattered because triage rules need to be predictable before a caller reaches the system, especially when a safety decision could route someone away from booking and toward emergency help.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_fbq9y250)

### The signed attribution rule that makes the ledger the contract

A job counted as the agent’s only when the caller’s number matched a completed CRM job within 7 days of the agent-created record and the job was verified against bank deposits.

That rule had to be agreed before the system created revenue. After money lands, both sides have a reason to read ambiguity in their own favor. I would want to credit the agent, and the client would want to credit reputation or prior relationship.

Defining attribution early made the ledger auditable instead of negotiable. That is why Decision §5 said not to argue attribution afterward.

## Setting Up the Project Infrastructure

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_kmf7hcgd)

### Verifying connectivity across Vapi, Twilio, and Airtable

The ngrok path was verified. The public HTTPS round trip returned HTTP 200 with the expected JSON.

Gemini was not verified. The check only confirmed that the key string existed, and a real call returned HTTP 400 with an invalid key.

Vapi, Twilio, Anthropic, and Airtable were not verified because no accounts existed. That meant the infrastructure still had design coverage, but not live service connectivity across those systems.

## Building and Testing the Voice Agent Triage Gate

### Wiring the agent prompt, tool schemas, and webhook handler

In this step, I built the voice-agent triage gate for the SA Plumbing workflow. The work centered on the Vapi Assistant prompt, the AI disclosure opener, the tool schemas, and the Express webhook handler.

The assistant prompt put triage before capture. The caller had to pass the safety screen before the agent could move into routine booking.

The webhook handler was still a stub. It logged tool calls and returned placeholder results, which gave the call flow a test surface before real Twilio, Airtable, and paging integrations existed.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_1rttlum9)

### Why a gas smell routes to an emergency page, never a booking

No live agent had actually heard a gas report. No number was provisioned because Vapi returned 403, no browser test was run, and no model processed that phrase in a real call.

What existed was the design. The triage gate ran before the capture flow, so a gas report should trigger the safety script: leave the house, do not touch switches, and call the gas company or 911. It should then page the on-call contact and not proceed to booking.

Booking should lose because the prompt gates capture behind cleared triage, and Decision §2 requires ambiguity to fail closed to a human. That is the intended behavior, not an observed production result, because the live model path remained untested.

## Connecting Real Systems: CRM, SMS Paging, and the Value Ledger

### Making every call branch write to a real system

In this step, I replaced the stub direction with the real-system integration plan. Emergency branches needed to trigger Twilio SMS alerts for on-call paging.

Routine branches needed idempotent writes to Airtable so jobs could be booked without duplicate records when retries occurred. Caller confirmations also had to be sent after successful booking.

The value ledger recorded every call disposition. That gave the system a 30-day review trail for attribution, disputes, and revenue measurement.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_zdgaqn23)

### Idempotent CRM writes and why replay safety matters

The CRM write used CallSID as the replay-safety key. Airtable used performUpsert with fieldsToMergeOn: ["CallSID"], and SQLite used UNIQUE(CallSID) with ON CONFLICT DO UPDATE.

That mattered because Vapi can retry tool calls when a network hiccup or timeout occurs. Without idempotency, one real call could create duplicate jobs, duplicate dispatches, and duplicate ledger rows that inflate attributed value.

The test proved the fix. Three identical retries produced one Jobs row and one Ledger row. Before the fix, the same retry pattern produced three Jobs rows and three Ledger rows.

## Proving the System Works: 45-Call Seeded Acceptance Test

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/f95effcd-cd0e-42e5-bd4f-b37d9abe598c_r49xtrav)

### Why a single emergency miss blocks the entire release

The acceptance criteria were strict. Emergency calls had to escalate 12 out of 12. Routine calls needed at least 18 out of 20 booked with correct fields. Ambiguous or prank calls had to create 0 out of 8 jobs. Ask-for-person calls needed 5 out of 5 transfer or callback outcomes. Replayed calls had to remain one job, and first response had to stay under about one second.

A single emergency miss blocked the release because the emergency category was a life-safety gate. Routine failures can lose revenue, and false bookings can waste a truck, but an emergency miss can leave a caller thinking help is coming when nobody was paged.

That failure mode is worse than no service. It creates false confidence during a gas leak, burst pipe, electrical risk, or no-heat situation. That is why emergency escalation had zero tolerance, and why the current 5 out of 12 result blocked the release.

## Reflections and Key Takeaways

### Tools and concepts from voice AI agent engineering

The key tools I used included Vapi for voice orchestration, Anthropic for AI reasoning, Twilio for telecommunications and SMS notifications, Airtable as the CRM database, and Linear for delivery tracking.

The main concepts I learned included call-log auditing, revenue-leak modeling, emergency-first triage, no-quoting behavior, attribution ledgers, idempotent writes, and field-by-field acceptance testing for voice agents.

The larger lesson was that a voice agent for trades work has to earn trust before it earns revenue. It must know when to book, when to page, when to refuse quoting, and when to hand off to a human.

### Time investment and biggest challenges

This build took me approximately 90 minutes. That time covered the call-log audit, decision documents, triage taxonomy, service-connectivity checks, prompt and tool design, webhook stubs, CRM and ledger logic, and seeded acceptance testing.

The hardest part was calibrating the triage gate so emergency calls were never treated like routine bookings. A single miss was unacceptable because the caller could face a safety issue, not just a scheduling inconvenience.

The biggest honest constraint was that several live services were not verified. ngrok worked, but Vapi, Twilio, Anthropic, and Airtable did not have live account verification in this build. The release was also blocked because emergency escalation only reached 5 out of 12 in the seeded test.

I completed this build to learn how to design an after-hours voice agent that combines triage, CRM integration, emergency paging, and an auditable value ledger. Next, I want to build multi-agent orchestration for more complex service workflows where dispatch, quoting, follow-up, and account review each have their own controlled role.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/f95effcd-cd0e-42e5-bd4f-b37d9abe598c)*
