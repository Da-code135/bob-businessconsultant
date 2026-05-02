# Discovery Questions

One question at a time. Adapt phrasing to user type. Wait for full answer before proceeding.

---

## Q1 — Problem
- **Non-tech:** "What does your business do, and what problem are you hoping software can solve for you?"
- **Tech:** "What system are you designing and what is the core problem it needs to solve?"

Listen for: sector, pain point, new vs existing system
Follow up if vague: "Can you walk me through what happens today — what process are you trying to fix?"

---

## Q2 — Users
- **Non-tech:** "Who will use this — your own staff, your customers, or both?"
- **Tech:** "Who are the end users — internal, external, or mixed? Different roles with different access?"

Listen for: internal vs external, roles, user count
Follow up: "How many people would log in on a typical day?"

---

## Q3 — Device
- **Non-tech:** "Will people use this on their phones, a computer, or both?"
- **Tech:** "Target platform — mobile, web, desktop, or combination? Native vs web-based preference?"

Listen for: mobile / desktop / both, field vs office use
Follow up: "Will they mostly be at a desk or moving around?"

---

## Q4 — Scale
- **Non-tech:** "On your busiest day, how many people might be using this at the same time?"
- **Tech:** "Expected concurrent user load at peak? Significant growth anticipated?"

Listen for: Low (<50) / Medium (50–500) / High (500+), growth trajectory
Follow up: "Busy Monday morning — how many people on the system at the same moment?"

---

## Q5 — Offline
- **Non-tech:** "Does this need to keep working when the internet goes down?"
- **Tech:** "Offline requirement? Full offline with sync, or graceful degradation?"

Listen for: hard offline / partial / always online
Follow up: "How reliable is the internet where staff or customers will use this?"

---

## Q6 — Data Sensitivity
- **Non-tech:** "What kind of information will this store — payments, personal details, medical records, or general business data like stock and orders?"
- **Tech:** "Data classification — financial, PII, health, or standard operational? Compliance requirements?"

Listen for: financial / PII / health / standard business / public
Follow up: "Would it be a serious problem if someone unauthorised accessed this data?"

---

## Q7 — Integrations
- **Non-tech:** "Does this need to connect to anything you already use — WhatsApp, Mobile Money, or any other tool?"
- **Tech:** "Third-party integrations required — payment gateways, messaging, internal systems, external APIs?"

Listen for: MTN MoMo / Airtel Money / WhatsApp / SMS / existing ERP or POS
Follow up: "Any tool your business already uses that this would need to talk to?"

---

## Q8 — Budget
- **Non-tech:** "Do you have a rough budget, or a preference for free tools vs paid ones?"
- **Tech:** "Budget constraints for infrastructure and licensing? Existing stack to align with?"

Listen for: open source only / mixed / flexible / specific tech preferences
Follow up: "Is keeping monthly costs low a priority, or is budget not the main concern?"

---

## Transition
After Q8, say: "Thank you — that gives me a clear picture. Let me put together a summary for you to review before I produce the full design."

Then run `03-summary_format.md`.