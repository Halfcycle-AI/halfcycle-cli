# Terms of Service — Halfcycle Connect

**Status.** **Reviewed and cleared** — legal review returned approved as it stands, with no edits owed
(T-15, 2026-08-19). Three post-approval fills applied without a separate legal pass — see the `1.0.0`
row in Version history. Published (T-26).
**Version.** `1.0.0`
**Effective date.** 2026-08-19. Hosted at
[`Halfcycle-AI/halfcycle-cli`](https://github.com/Halfcycle-AI/halfcycle-cli/blob/main/docs/legal/terms-of-service.md).
**Scope.** **Connect only.** The portal has no separate terms of service in this phase — decision 8,
`docs/phases/phase-connect-signup-and-pilot-launch.md`. The portal's data handling is covered by the
privacy policy (`privacy-policy.md`), section 2, but this terms document does not apply to it.
**Phase.** Connect signup and pilot launch, W-7 (`docs/phases/phase-connect-signup-and-pilot-launch.md`).

**Contracting entity.** Halfcycle AI, Inc., a Delaware corporation. Sections 9 and 10 are written for
this entity. **Notice address for TOS-DIS-1.** hello@halfcycle.ai.

**Defined term.** "**the Service**" below means Halfcycle Connect — the guard engine, the hosted guard
evaluation and method delivery endpoints, the control plane, and anything served through them.
"**Output**" means any guard verdict, method fragment, readiness signal, report or other material the
Service returns to you.

---

## 1. What this covers

Halfcycle Connect runs a guard engine in your own repository, on your own machine, and reaches
Halfcycle over the network for guard evaluation, method delivery and engagement lifecycle
(`docs/connect-wire-payloads.md` §1). By installing or using Connect you agree to these terms.
Declining, or later withdrawing agreement, does not delete your account — it blocks product access
until you accept (see `privacy-policy.md` §1.6 for what account deletion does and does not do).

## 2. Anti-extraction — what you may not do

The serve tier is leak-resistant by architecture (stage-scoped fragments, no enumerable index); these
clauses make the remaining attack contractually prohibited. **Each clause carries a stable identifier.**
Legal review will rewrite the prose below; the identifier is what stays stable across that rewrite, and
what any future reference (a term in another document, a support ticket, an enforcement notice) should
cite.

### TOS-AX-1 — No enumeration or scraping
You may not attempt to enumerate, harvest, scrape or systematically retrieve served method content
beyond what the current step of your engagement requires.

### TOS-AX-2 — No probing to infer protected logic
You may not probe, fuzz, reverse-engineer or otherwise interrogate the API to infer guard rules,
matchers, rubrics, question sets, or verdict-tier logic (INV-007 — the client surface never exposes
the asset; this clause protects what INV-007 keeps out of the client-visible response).

### TOS-AX-3 — No automated access outside normal use
You may not use automated or scripted access designed to elicit responses outside normal product use,
including replaying an engagement's state to sweep steps out of order.

### TOS-AX-4 — No circumventing metering or rate limits
You may not circumvent, disable or tamper with quota, metering or rate limits.

### TOS-AX-5 — No redistribution
You may not share, resell or redistribute served content or credentials issued to you.

### TOS-AX-6 — Enforcement is reserved, not a claim of an existing detector
Halfcycle reserves the right to throttle, suspend or terminate access on detection of conduct
prohibited by TOS-AX-1 through TOS-AX-5. **This is a contractual posture, not a description of a
technical control that exists today.** No automated anomaly or abuse detector is implemented as of
this document's version (independently confirmed against the codebase, 2026-08-17; corroborated by
`docs/connect-launch-readiness.md`). Nothing in this document should be read as claiming otherwise.
Reserving the right does not require building the mechanism first, but this clause must not be
represented — here or elsewhere — as describing a control that is live.

## 3. Account and acceptance

Both this document and the privacy policy are versioned. Acceptance is recorded against your account
together with the version you accepted (mechanism: `docs/features/connect-signup-and-credential.md`
and the acceptance-recording task, not built by this document). When either document's version
changes, you are asked to accept again; declining keeps your account and blocks product access —
nothing about your account or its data is deleted on a decline.

## 4. No data residency promise

This document, and the privacy policy, say nothing about where your data is processed beyond what is
configured per engagement. Region is per-engagement configuration; no public copy promises residency
by default (INV-012, `docs/invariants.md`).

## 5. What you are responsible for

**Sections 5 through 8 exist because of one fact about this product: Halfcycle does not know what you
are specifying or building.** What the Service receives is descriptors of a change — file paths,
structural shapes, environment variable names, route signatures and similar — together with whatever
you choose to record as evidence. What it does not receive is your requirements, your design intent,
your data, your users, your commercial context or your legal obligations, and nothing in the Service
puts Halfcycle in a position to judge whether what you are building is correct, safe, lawful or fit for
your purpose. The allocation of risk below follows from that.

### TOS-RSP-1 — You decide what you build
You alone choose your requirements, specifications, architecture, dependencies, data handling and
release decisions. Halfcycle does not review, approve, certify or supervise your software, and has no
visibility into it beyond the descriptors named above.

### TOS-RSP-2 — Output is advisory, never a certification
Output is informational input to your own engineering judgment. It is not a certification, audit,
attestation, approval or sign-off of correctness, security, safety, performance, licensing,
accessibility or regulatory compliance. A guard that passes does not mean your software is correct; a
guard that fails does not mean it is broken. No Output creates any professional, advisory or fiduciary
relationship, and none of it is legal, regulatory, security, accounting or financial advice.

### TOS-RSP-3 — Independent review before reliance
Output is generated in part by machine-learning systems that are probabilistic and non-deterministic:
they can be inaccurate, incomplete, outdated, internally inconsistent or confidently wrong, and the
same input can produce different Output on different occasions. You will independently review, test and
verify Output before relying on it, and you are solely responsible for every decision you make and
every action you take on the basis of it.

### TOS-RSP-4 — Your inputs
You represent that you hold all rights, consents and licences necessary to submit what you submit; that
submitting it breaches no law and no third party's rights; and that you will not submit **the personal
data of others**, protected health information, payment card data, government-classified material or
export-controlled technical data except where Halfcycle has agreed to receive it in writing. This does
not restrict the account information you necessarily provide by signing in and using the Service — your
identity-provider identifier, your email address and the usage records described in `privacy-policy.md`
§1.5 and §1.8 — which Halfcycle asks for, holds and deletes on the terms that policy sets out.

### TOS-RSP-5 — High-risk uses
The Service is not designed, tested or authorised for use in circumstances where failure could lead to
death, personal injury, or severe physical, environmental or property damage — including medical
devices and clinical decision-making, aviation or vehicle control, nuclear facilities, life support,
weapons systems, emergency services and critical infrastructure — nor for automated legal, credit,
employment, insurance or immigration determinations affecting individuals without qualified human
review. If you use it that way you do so entirely at your own risk and take sole responsibility for the
consequences.

### TOS-RSP-6 — Your end users
Anything you build, ship, license or operate is yours, offered in your own name and under your own
terms. Halfcycle has no contractual or other relationship with your customers, end users, employees or
contractors, and makes no representation to them.

## 6. Warranty disclaimer

### TOS-WAR-1 — Provided as is
**The Service and all Output are provided "AS IS" and "AS AVAILABLE", with all faults and without
warranty of any kind.**

### TOS-WAR-2 — Warranties disclaimed
To the maximum extent permitted by law, Halfcycle disclaims all warranties, conditions and
representations, whether express, implied, statutory or arising from course of dealing, course of
performance or usage of trade — including any implied warranty of merchantability, fitness for a
particular purpose, title, non-infringement, quiet enjoyment, accuracy, workmanlike effort, system
integration or uninterrupted use. Where a jurisdiction does not permit the exclusion of an implied
warranty, that warranty is limited to the shortest period and narrowest scope that jurisdiction allows.

**This clause does not disclaim the commitments Halfcycle makes to you in the privacy policy.** Those
are express undertakings about what Halfcycle does with your data — what the guard path sends, what is
not used for training, what deletion deletes — and they stand on their own terms, including where the
policy labels one a forward commitment rather than present behaviour. A warranty disclaimer that
silently swallowed the privacy policy would make the policy unenforceable by the person it was written
for, which is not what this clause is for.

### TOS-WAR-3 — No warranty as to Output
Halfcycle does not warrant that Output will be accurate, complete, current, reliable, reproducible,
deterministic, unbiased, free of error, or free of any third party's intellectual property or other
rights, nor that it will identify any particular defect, vulnerability, licence conflict or compliance
gap in your software. Detection is a matter of what the guards look for; **absence of a finding is not
a finding of absence.**

### TOS-WAR-4 — No availability commitment
Halfcycle does not warrant that the Service will be available, uninterrupted, timely, secure or free
of data loss, and **this document makes no service-level, uptime, support-response or backup
commitment.** Access, features, quotas and pricing may be modified, suspended or discontinued at any
time. If a service-level agreement is ever offered it will be a separate signed document, and nothing
here should be read as one existing today. What Halfcycle does and does not retain, and what deletion
deletes, is governed by `privacy-policy.md` and is not disclaimed here.

### TOS-WAR-5 — No compliance or certification warranty
Halfcycle does not warrant that the Service or any Output will satisfy any legal, regulatory,
contractual or industry requirement that applies to you or to what you build — including GDPR, PIPEDA,
HIPAA, PCI DSS, SOC 2, ISO 27001, the EU AI Act, sectoral financial rules, accessibility standards or
export-control law. **No DPA, sub-processor commitment, audit right, certification or compliance
artefact is offered under this document** (`docs/features/connect-terms-and-data-rights.md`, Non-goals).

### TOS-WAR-6 — Third parties
Output may be produced with the assistance of third-party model providers and infrastructure providers.
Halfcycle gives no warranty in respect of any third-party service and is not responsible for the acts,
omissions, availability, changes or discontinuation of any third party. Halfcycle's own commitments
about what those providers may do with your content live in `privacy-policy.md` §3, and **this clause
neither enlarges nor cuts into them** — it disclaims warranties about a third party's service, not the
undertaking Halfcycle gives you about how content is sent to it.

### TOS-WAR-7 — Beta and pilot access
Any access identified as pilot, beta, preview, trial or evaluation is provided for evaluation only, may
contain defects, and may be withdrawn without notice. Sections 6 through 8 apply to it in full.

## 7. Limitation of liability

### TOS-LIA-1 — No indirect or consequential damages
To the maximum extent permitted by law, Halfcycle and its affiliates, officers, directors, employees,
contractors, agents, licensors and suppliers will have no liability for any indirect, incidental,
special, consequential, exemplary or punitive damages, or for any loss of profits, revenue, goodwill,
business, opportunity, anticipated savings or data; business interruption; cost of procuring substitute
goods or services; corruption, inaccuracy or unavailability of data; or damages arising out of the
software, products or services **you** design, build, ship or operate, out of any decision made in
reliance on Output, or out of any claim brought against you by a third party. This applies regardless
of the theory of liability — contract, tort (including negligence), strict liability, statute or
otherwise — **even if Halfcycle has been advised of the possibility of such damages, and even if a
limited remedy is found to have failed of its essential purpose.**

### TOS-LIA-2 — Aggregate cap
To the maximum extent permitted by law, the total aggregate liability of Halfcycle and the parties
named in TOS-LIA-1, for all claims arising out of or relating to these terms or the Service, will not
exceed the greater of (a) the total fees you actually paid to Halfcycle for the Service in the twelve
months immediately preceding the first event giving rise to the claim, and (b) **fifty United States
dollars (US$50)**.

> **Why a floor rather than fees alone.** Connect is provided without charge today, so a fees-only cap
> would be **US$0** — the tightest cap available, and the reason it is not used here: a zero cap invites
> a court to strike the whole clause as illusory and leaves you with an uncapped exposure instead of a
> small one. US$50 is the lowest figure in common use that keeps the clause standing. This was the
> instruction ("whatever is the least") applied, not overridden — flagged so legal review sees the
> reasoning rather than re-deriving it.

### TOS-LIA-3 — One cap, not one per claim
The cap in TOS-LIA-2 is a single aggregate limit across all claims, causes of action and claimants.
Multiple claims do not enlarge it.

### TOS-LIA-4 — Time limit on claims
Except where a longer period is required by law, any claim arising out of or relating to these terms or
the Service must be brought within **one (1) year** after the claim first accrues; a claim brought after
that period is permanently barred.

### TOS-LIA-5 — Basis of the bargain
Sections 6 and 7 allocate risk between the parties and are reflected in the price of the Service,
including its being offered without charge. Halfcycle would not provide the Service on these terms
without them. They apply even where an exclusive or limited remedy fails of its essential purpose, and
they survive termination.

### TOS-LIA-6 — What is not limited
Nothing in these terms excludes or limits liability that cannot lawfully be excluded or limited —
including liability for death or personal injury caused by negligence, for fraud or fraudulent
misrepresentation, and, in jurisdictions where it may not be excluded, for gross negligence or wilful
misconduct. Some jurisdictions do not allow certain exclusions or limitations, so parts of sections 6
and 7 may not apply to you; where that is so, they apply to the maximum extent permitted.

## 8. Indemnification

### TOS-IDM-1 — Your indemnity
You will defend, indemnify and hold harmless Halfcycle and its affiliates, officers, directors,
employees, contractors and agents from and against any third-party claim, demand, action, proceeding,
investigation or regulatory enquiry, and all resulting losses, damages, liabilities, judgments,
settlements, fines, penalties, costs and reasonable legal fees, arising out of or relating to:

(a) your inputs, repositories, specifications, configuration and credentials;
(b) the software, products or services you design, build, deploy, license or distribute, in whole or in
    part with the assistance of the Service, and any defect, failure, outage, breach or harm they cause;
(c) your use of or reliance on any Output, including any decision made or omitted on the basis of it;
(d) your breach or alleged breach of these terms, including TOS-AX-1 through TOS-AX-5;
(e) your violation of any law or of any third party's rights, including intellectual property, privacy,
    data-protection and export-control rights; and
(f) any claim brought by your customers, end users, employees or contractors.

### TOS-IDM-2 — Procedure
Halfcycle will notify you of a claim without undue delay (a delay relieves you only to the extent it
materially prejudices your defence), give you control of the defence and settlement of that claim, and
provide reasonable cooperation at your expense. You may not settle a claim in a way that admits fault
by Halfcycle, imposes any non-monetary obligation or payment on Halfcycle, or fails to release it
unconditionally, without Halfcycle's prior written consent. Halfcycle may participate with counsel of
its own choosing at its own expense.

**If you do not assume the defence promptly**, or you stop conducting it diligently, or you become
insolvent, Halfcycle may take over the defence and settle the claim on commercially reasonable terms,
and you remain liable under TOS-IDM-1 for the resulting costs, damages and settlement amounts.

**Halfcycle retains control of any regulatory enquiry, investigation or enforcement proceeding directed
at it**, because those cannot practically or lawfully be conducted by someone else in its name; your
indemnity under TOS-IDM-1 applies to them, your control under this clause does not.

### TOS-IDM-3 — Not capped by section 7
Your obligations under TOS-IDM-1 are not subject to the limits in section 7.

### TOS-IDM-4 — Halfcycle gives no indemnity under this document
**Halfcycle provides no indemnity of any kind under these terms, including no intellectual-property
indemnity for Output.** This is stated plainly rather than left to silence, because silence in a terms
document is routinely read as an oversight to be argued about later. It follows from the posture of a
free, self-serve pilot in which Halfcycle cannot see what you build. A negotiated enterprise agreement
may provide an indemnity; this document does not, and **expect this clause to be the first thing an
enterprise counterparty asks to change.**

## 9. Governing law and venue

### TOS-LAW-1 — Governing law
These terms, and any dispute arising out of or relating to them or to the Service, are governed by the
laws of the **State of Delaware, United States**, without regard to its conflict-of-laws rules. The
United Nations Convention on Contracts for the International Sale of Goods and the Uniform Computer
Information Transactions Act do not apply.

### TOS-LAW-2 — Venue
Subject to section 10 — and in particular to the small-claims carve-out in TOS-DIS-4(a), which is not
confined to this venue — the state and federal courts located in **New Castle County, Delaware** have
exclusive jurisdiction over any dispute that is not required to be arbitrated. Each party consents to
personal jurisdiction there and waives any objection based on inconvenient forum.

### TOS-LAW-3 — Language
These terms are drafted in English. Any translation is provided for convenience; the English text
governs.

## 10. Dispute resolution

### TOS-DIS-1 — Informal resolution first
Before starting an arbitration or a court proceeding, the party with the complaint will send a written
notice describing the dispute and the relief sought to the other party — to Halfcycle at the notice
address published with this document — and the parties will try in good faith to resolve it for
**thirty (30) days**. Any limitation period is tolled while that runs. **This clause does not apply to
anything in TOS-DIS-4**: a party may go to small-claims court, or apply for injunctive or other
equitable relief, immediately and without waiting. A mandatory cooling-off period in front of a
restraining order would defeat the relief it exists to obtain.

### TOS-DIS-2 — Binding individual arbitration
If the dispute is not resolved under TOS-DIS-1, it will be finally resolved by **binding arbitration**
administered by the American Arbitration Association under its Commercial Arbitration Rules, before a
single arbitrator, seated in **New Castle County, Delaware**, conducted in English, and — where the
rules allow — on written submissions or by videoconference. The arbitrator decides all questions of
arbitrability and of the interpretation, applicability, enforceability and formation of this section,
**except that a court of competent jurisdiction, and not the arbitrator, decides whether TOS-DIS-3 (the
class-action and jury waiver) is enforceable** — that question is expressly reserved to a court.
Judgment on the award may be entered in any court of competent jurisdiction. The Federal Arbitration
Act governs the interpretation and enforcement of this section.

### TOS-DIS-3 — Class action and jury waiver
**Claims may be brought only in an individual capacity, and not as a plaintiff or class member in any
class, collective, consolidated, coordinated or representative proceeding, including any private
attorney general action.** The arbitrator may not consolidate the claims of more than one party or
preside over any representative proceeding. **Each party waives any right to a jury trial.** If this
paragraph is held unenforceable as to a particular claim, that claim — and only that claim — is severed
from the arbitration and heard in the courts named in TOS-LAW-2; the remainder proceeds in arbitration.

### TOS-DIS-4 — Carve-outs
Either party may (a) bring an individual claim in any small-claims court of competent jurisdiction over
that party, if the claim qualifies and stays in that court, and (b) apply to
the courts named in TOS-LAW-2 for temporary, preliminary or permanent injunctive or other equitable
relief to prevent actual or threatened infringement, misappropriation or violation of intellectual
property or confidential information — including conduct prohibited by TOS-AX-1 through TOS-AX-5.
Seeking such relief is not a waiver of the right to arbitrate anything else.

### TOS-DIS-5 — Opt-out
You may opt out of TOS-DIS-2 and TOS-DIS-3 by sending written notice to the address in TOS-DIS-1 within
**thirty (30) days** of first accepting these terms, identifying your account — by the email address it
signs in with — and stating your intent to opt out. Opting out affects nothing else in this document;
disputes then go to the courts named in TOS-LAW-2.

### TOS-DIS-6 — Costs
Arbitration fees are allocated under the applicable AAA rules. Each party bears its own legal fees
unless the arbitrator awards otherwise under applicable law.

### TOS-DIS-7 — Survival and severability
**Section 2 (TOS-AX-1 through TOS-AX-6) and sections 5 through 10 survive** termination or expiry of
these terms, and any suspension, termination or deletion of your account. Section 2 is named explicitly:
the prohibitions on enumeration, probing, circumvention and redistribution are most likely to be tested
by someone whose access has just been terminated, and TOS-IDM-1(d) and TOS-DIS-4(b) both operate on
them. If any provision is held unenforceable, it is limited or severed to the minimum extent necessary
and the rest remains in force.

## 11. Open items for legal review

Named here rather than resolved, because resolving them means facts this repository does not hold or
judgments a lawyer must make.

- **Non-US and consumer counterparties.** Class-action waivers and pre-dispute arbitration clauses are
  unenforceable against consumers in Ontario and Quebec, and consumer-protection and unfair-terms rules
  in the EU and UK cut into sections 6, 7 and 10. This document assumes **business counterparties only**.
  If self-serve signup can produce an individual consumer — and today nothing gates it — that assumption
  needs either a gate in the product or a jurisdiction-specific rider.
- **AAA versus JAMS**, and whether a fee-shifting or consumer-arbitration protocol should be named.
- **A super-cap for carve-outs.** TOS-LIA-2 is a single low cap with no higher tier for indemnity,
  confidentiality or anti-extraction breach. Enterprise counterparties usually ask for one.
- **TOS-IDM-4 (no Halfcycle indemnity)** — confirm this is the intended commercial posture for the pilot
  and that sales knows it is in the document.
- **Export control and sanctions.** No clause here restricts use by sanctioned parties or in embargoed
  jurisdictions. Standard for US-entity terms; deliberately not invented in this pass.
- **Acceptable use beyond anti-extraction.** Section 2 protects the serve tier. There is no general AUP
  (no clause on unlawful content, security testing against Halfcycle, or resale). Out of this pass's
  scope; flagged so its absence is a decision rather than an omission.
- **Termination and suspension mechanics** beyond what §3 and TOS-AX-6 state — notice, effect, wind-down,
  data export — remain undrafted.

## Non-goals

- **Portal terms of service** — explicitly out of scope for this document (decision 8). The portal is
  covered only by the privacy policy's data-handling sections.
- Cookie banners, marketing-site compliance, a DPA, sub-processor list, SOC 2, or per-jurisdiction
  variants — out of scope for this phase (`docs/features/connect-terms-and-data-rights.md`, Non-goals).
- Building the detection mechanism TOS-AX-6 reserves rights against.
- A general acceptable-use policy, export-control clause, or termination mechanics — see section 11.

## Version history

| Version | Date | Note |
|---|---|---|
| `1.0.0` | 2026-08-19 | Legal review complete (T-15): approved as it stands, no edits owed. **No substantive clause changed** between `0.2.0-draft` and this version — an acceptance recorded against either version string is an acceptance of the same agreement. Three changes, all non-substantive: (1) the contracting entity is resolved — **Halfcycle AI, Inc., a Delaware corporation**, notice address for TOS-DIS-1 is `hello@halfcycle.ai` — replacing the "UNRESOLVED" banner and the `[Halfcycle, Inc.]` placeholder that sat under it; (2) the §5 parenthetical marked "Drafting note, for review rather than for publication" is removed, and the paragraph it sat under is unchanged; (3) §11's now-closed "contracting entity" open item is removed, the rest of §11 is unchanged. The `-draft` qualifier and the "requires a lawyer's pass" status are dropped. **In force as of 2026-08-19** — hosted at `Halfcycle-AI/halfcycle-cli` (`docs/legal/terms-of-service.md`, T-26); no text changed to publish it. |
| `0.2.0-draft` | 2026-08-19 | §5 placeholder replaced with drafted substance: responsibility (TOS-RSP), warranty disclaimer (TOS-WAR), limitation of liability (TOS-LIA), indemnification (TOS-IDM), governing law and venue (TOS-LAW), dispute resolution (TOS-DIS). Founder decisions: Delaware law and venue; arbitration with class waiver, small-claims and injunctive carve-outs and a 30-day opt-out; liability cap the greater of 12-month fees or US$50. Remaining gaps enumerated in §11 rather than guessed at. Still not legal text. **A structural review pass on this draft corrected ten defects before it was written down** — including a §5 preamble that re-imported the absolute source claim `privacy-policy.md` §1.2 repudiates; a warranty disclaimer broad enough to swallow the privacy policy's own express commitments; a personal-data prohibition every user breached at signup; a 30-day cooling-off period that disabled the injunctive carve-out it sat beside; an arbitrability delegation whose exception pointed at nothing; a small-claims carve-out exercisable only in Delaware; a survival clause that dropped section 2; and an indemnity procedure with no failure-to-defend path. |
| `0.1.0-draft` | 2026-08-17 | Initial substance draft (T-14). Not published. Pending legal review. |
