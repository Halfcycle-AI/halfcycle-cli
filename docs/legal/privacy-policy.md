# Privacy Policy — Halfcycle

**Status.** **Reviewed and cleared** — legal review returned approved as it stands, with no edits owed
(T-15, 2026-08-19). Two post-approval fills applied without a separate legal pass — see the `1.0.1` row
in Version history. Published (T-26).
**Version.** `1.0.1`
**Effective date.** 2026-08-19. Hosted at
[`Halfcycle-AI/halfcycle-cli`](https://github.com/Halfcycle-AI/halfcycle-cli/blob/main/docs/legal/privacy-policy.md).
**Scope.** **Both surfaces**, in two clearly separated sections, because they genuinely differ
(decision 8, `docs/phases/phase-connect-signup-and-pilot-launch.md`): §1 covers **Connect**, whose
guard path sends no source; §2 covers the **portal**, whose guided authoring persists everything
submitted. Nothing below states one averaged claim about both.
**Phase.** Connect signup and pilot launch, W-7 (`docs/phases/phase-connect-signup-and-pilot-launch.md`).

**How to read this document.** Every factual claim below about what the system does carries an
anchor — a citation to a captured wire payload or to the code that makes the claim true. Where no such
anchor exists because the behaviour is not built yet, the claim is explicitly labelled **a
commitment**, written so it cannot be mistaken for something already shipped. A consolidated anchor
table is at the end of this document (§5).

---

## 1. Connect

Connect runs the guard engine in your own repository, on your own machine. It reaches Halfcycle over
HTTPS for three purposes: engagement lifecycle, the method delivery loop, and guard evaluation
(`docs/connect-wire-payloads.md` §1).

### 1.1 What Connect sends

The guard evaluation call — the one call in Connect that inspects your code — sends **descriptors of
a change, not its content**. There is no `content` field, no `diff` field, and no file body in the
request Connect's guard hook sends. What travels is file paths, structural shapes (`astShapes`),
environment variable names and how they're used, table/verb pairs for SQL writes (never values),
route signatures, constraint kinds, numeric column declarations, and outbound host names.
*(Anchor: `docs/connect-wire-payloads.md` §2, captured live from a running deployment, commit
`e5da212`, method `0.9.12`.)*

### 1.2 Source excerpts — the exact guarantee, and why it has two halves

The wire format has a `sourceExcerpts` field that is built and reachable — it is not hypothetical — so
the true statement about your source and this product is conditional, not absolute, and the two
conditions that make it true today live at different ends of the wire:

- **On your machine, the runner does not attach source excerpts to a guard evaluation unless you
  explicitly turn a flag on, and that flag is off by default.** *(Anchor:
  `packages/runner/src/config.ts` — `readExcerptConfig()`, gated on `HALFCYCLE_SOURCE_EXCERPTS`,
  default off.)*
- **On Halfcycle's side, any excerpt that does arrive for an engagement that has not opted in is
  stripped before the guard engine or any model sees it.** *(Anchor:
  `services/guard/src/routes/evaluate.ts` — the opt-in gate around evaluation, which strips
  unauthorized excerpts rather than failing open on a read error.)*
- **Today, no engagement can opt in.** The store column that would enable excerpts
  (`engagements.source_excerpt_optin`) has a setter, but that setter has no caller outside tests — no
  route in the product can flip it. *(Anchor: `services/control/src/engagements/engagement-store.ts`
  defines `setSourceExcerptOptIn`; `services/control/src/engagements/index.ts` re-exports it; nothing
  else calls it. **Cited by symbol rather than by line number, deliberately** — this anchor has been
  written as `:204`, then `:248`, then `:314` in the space of a week as unrelated work grew the file,
  which is three wrong citations for a fact that never changed. The symbol is what a reader should
  search for; the line number is a footgun that makes a true claim look stale.)*

**The precise claim, stated correctly:** source excerpts are off for every Connect engagement today,
by construction on both ends, and there is currently no way — for you or for Halfcycle — to turn them
on. If that ever changes, this section changes with it, because the guarantee above is what makes it
true, not a general intention.

### 1.3 What is not sent

Stated positively, because absence is easy to overclaim from examples alone: no file contents (source
excerpts are off, per §1.2); no repository clone — Halfcycle has no filesystem access to your
repository; no credentials in a URL; no cross-engagement reads of your data. *(Anchor:
`docs/connect-wire-payloads.md` §6.)*

### 1.4 Verdict-checking content — a commitment, not a live feature

Halfcycle has decided, and is publishing that decision now rather than waiting for the feature to
ship: **content submitted to a future readiness or consistency verdict route will be evaluated and
not retained** — what will persist is the intervention record, carrying evidence (an anchor, a diff,
or a question-and-answer), never the source itself. Training or benchmarking on submitted client
content is deliberately foreclosed. *(Anchor: `docs/connect-product-spec.md` §8 item 8 — a decision,
not shipped behaviour.)*

**This is explicitly labelled as a forward commitment, scoped to Connect, because the feature does not
exist yet.** There is no verdict route in the product today, and no code writes to the
`verdict-calls` quota dimension. *(Anchor: `services/control/src/entitlement/quota.ts` —
`QUOTA_DIMENSIONS_AWAITING_A_WRITER` names `verdict-calls` as a dimension with a declared row and no
writer.)* Do not read this section as describing something you can use today; it describes what will
be true when it ships.

### 1.5 Quota and IP address

Connect meters usage — including guard evaluations, which are never refused, only recorded (decision
11) — and one dimension is keyed on where the request came from rather than on your account.
Specifically, engagement creation is counted per source **network**, because at the moment of creation
there is no engagement yet to key on. *(Anchor: `services/control/src/routes/engagements.ts` — the
`recordUsage({ kind: 'source-address', key: request.ip }, ...)` call on `POST /engagements`.)*

**What is stored is a network prefix, not your address.** The address the request arrives from is
truncated before it is written down — IPv4 to its first three octets (a `/24`), IPv6 to its first
three groups (a `/48`) — so the counter's key names a network shared by up to 256 machines rather than
a single one. *(Anchor: `services/control/src/entitlement/quota.ts` — `truncateSourceAddress`, applied
inside `recordUsage` to every `source-address` subject before any read or write uses the key.)* Rows
written before this behaviour shipped held the full address and were **deleted**, not shortened.
*(Anchor: `services/control/src/store/migrations/1788000000000_account-deletion.sql`.)*

**None of the metering tables carries a database link to an account or a project** — the foreign key
is deliberately absent from the tables, not merely from this one dimension, because a creation is
counted before the thing being created exists. That matters for deletion and §1.6 spells out the
consequence: counters keyed on your account or on one of your projects **are** deleted with you,
because the deletion goes and looks for them by name; the network-keyed counter is the one that cannot
be, because nothing is on the other end of the key. Truncating it where it is written is what makes
that survivable rather than a gap.

### 1.6 Account deletion

**Deleting your account deletes it — the account and the personal data linked to it.** This is built
and it runs today. *(Anchor: `services/control/src/accounts/account-deletion.ts` — `deleteAccount`,
one database transaction, so the outcome is always all of it or none of it.)*

**How you ask.** Ask us, at hello@halfcycle.ai, and we do it — the same address the terms of service
publish for TOS-DIS-1. There is **no self-serve delete button**,
because there is no account-settings page yet — the whole of Connect's signed-in surface today is the
sign-in confirmation step. We would rather say that plainly than describe a control you cannot find.
*(Anchor: `services/control/src/scripts/account-admin.ts`, and the operator procedure in
`docs/operations.md`. No route in the product deletes an account.)* When a settings page exists it will
call exactly the same mechanism, and this paragraph will change to name the button.

**What is deleted.** Your account row and every field on it — the sign-in identifier your identity
provider gave us, your email address, the date you signed up, and the date you last used the product
(see §1.8, which lists them). Every credential you hold: the long-lived credential on your machine,
any sign-in codes still in flight, and your per-project access credentials, *including* those for
projects owned by someone else. And every project you own **alone**, with everything recorded against
it — its guard runs, its phase history, its delivery records, its link to a source-control
installation, and, if you used the portal, the uploads, answers, transcript and drafted documents
§2.1 describes.

**What is deleted is not recoverable.** There is no undo and no grace period.

**What survives, stated in full rather than in the abstract.** Three things, and each is here because
it cannot honestly be promised away:

1. **A project you share with someone else is not deleted.** It stops being *yours* — your ownership
   of it is removed and so is your own access credential — but the project itself and the other
   person's access are untouched. Deleting your account cannot delete another person's work.
2. **Aggregate benchmark rows are kept, with the link to your project removed.** These are the
   per-project, per-phase summary figures Halfcycle uses to understand how the method performs across
   all engagements. The row survives; the reference identifying which project produced it is set to
   nothing, permanently, so no kept row points back at you and no kept row can be traced to your work
   again. This is a deliberate decision rather than an oversight, and it is stated here rather than
   settled quietly in a database migration.
3. **A name you typed into a shared project's decision log stays in that log.** If you recorded a
   phase decision on a project you share, the record carries the name you supplied at the time — a
   free-text value, not a reference to your account — so nothing connects it to the account being
   deleted and the deletion cannot locate it. It is also the other person's project history, which we
   will not rewrite. On projects you owned alone, these records are deleted with the project.

**And two things deletion cannot reach at all**, stated for the same reason:

- **The network-keyed usage counters in §1.5, and those only.** Be precise about which, because the
  metering tables hold several kinds of counter and only one of them is beyond reach. Counters keyed
  on your **account**, and counters keyed on a **project that is deleted with you**, go with you —
  they name you, and the deletion knows to look for them even though no database link points at them.
  A project you shared, which survives, keeps its own counters, because they are the other person's
  measure of a project that is still theirs. The counter keyed on a **network** is different again:
  nothing is on the other end of that key at all, so there is no way to find the rows that were
  yours. **This is the gap the previous version of this section promised to
  close, and this is how it is closed:** the stored key is truncated to a network prefix before it is
  ever written, so what survives names a network of up to 256 machines rather than your address. It is
  not a full address, and it was never linked to you.
- **A project created before sign-in existed.** Early Connect created projects with no owner at all,
  and no path was ever built to claim one. Such a project has no reference to any account, so nothing
  connects it to you — which is also why leaving it is not a retention of your personal data.

**One pattern runs through the last four points, and naming it is more honest than four separate
exceptions.** In each case the data survives *because nothing links it to you* — no reference from
that row back to your account for a deletion to follow.

**Missing link and unreachable are not the same thing, though, and the difference is the whole of what
this section can promise.** Several of the records above carry no database link to you and are still
deleted, because the deletion knows to go and look for them by name: your account-keyed usage
counters, and every counter belonging to a project deleted with you. Being unlinked makes a record
easy to miss; it does not make it unreachable. What is genuinely unreachable is the shorter list: a
counter whose key names a network and not a person, and a project that never had an owner.

**And being unlinked is not the same as being anonymous, which is where the decision log sits.** A
name you typed is not made anonymous by being unlinked — only unfindable. So this section says which
of the three applies to each item rather than folding them into one reassuring sentence about
aggregate data identifying nobody.

*(Anchors for this section: `services/control/src/accounts/account-deletion.ts` for the deletion graph
and its `ENGAGEMENT_FK_DISPOSITIONS` / `ACCOUNT_FK_DISPOSITIONS` maps, which name every table an
erasure touches and what happens to it; `services/control/test/account-deletion.test.ts`, which
asserts those maps against the live database schema in both directions, so a table added later cannot
be quietly skipped by a deletion that appears to have worked.)*

### 1.7 Guard, method-delivery and lifecycle content

Guard evaluation, the method-delivery loop, and engagement lifecycle calls carry only what
`docs/connect-wire-payloads.md` shows: engagement identifiers, step/command identifiers, phase
verdicts and evidence you record, and (per §1.1–§1.2) descriptor data about your changes, never
source. This is engagement-scoped data, not personal data about you beyond what your account holder
identity requires — §1.8 says exactly what that identity consists of.

### 1.8 Your Halfcycle account — the whole of what we hold about you

Signing in creates one account record. It has **five** fields and **four of them are personal data**;
they are listed individually because a policy that named only the obvious one would be understating
what is held. *(Anchor: `services/control/src/accounts/types.ts` — the `PersistedAccount` type, and
`accounts-and-ownership.sql`, which creates the table.)*

- **The sign-in identifier your identity provider gave us.** Not your email, and not a name — but it
  identifies you at that provider, it is the same value every time you sign in, and it resolves to you
  there. Pseudonymous is not anonymous, and we do not describe it as anonymous.
- **Your email address**, as your identity provider last reported it. We treat the provider as the
  source of truth and re-read it at each sign-in rather than pinning whatever you signed up with.
- **The date and time you created the account.**
- **The date and time you last used the product.** This is behavioural — it says when you were last
  here — and it is named separately because it is the field most easily mistaken for bookkeeping.
- The fifth field is an internal identifier we generate for the row. It is ours, it means nothing
  outside our database, and it is not information about you.

**And the account is not the whole picture, because it is linked.** A project you create records that
you own it, which is what ties a person to their work. *(Anchor: `engagements.owner_account_id`.)* Any
honest account of what is known about you has to include that link, not only the five fields above,
and §1.6 is written against the link as well as the fields.

**What we do not hold here:** no password (your identity provider handles sign-in, and we never see
one), no payment details, and no profile you filled in — there is no profile.

---

## 2. The portal (guided authoring)

The portal is a different surface with a different job: it walks you through drafting a product
specification, and to do that, it keeps what you give it. This section names exactly what, checked
against the tables that actually hold it — not what the product intends to store, what it does.

### 2.1 What the portal retains

- **Uploaded document text**, in full. *(Anchor: `authoring_uploads.full_text`,
  `services/control/src/authoring/authoring-store.ts:801` — column definition; `:845` — the insert
  that writes it.)*
- **Your typed answers to the authoring questions.** *(Anchor: `authoring_answers.answer_text`,
  `services/control/src/authoring/authoring-store.ts:59` — column definition; the upsert around it
  keeps the current answer, not a full edit history.)*
- **The full conversation transcript**, in the order it happened. *(Anchor:
  `authoring_transcript.text`, `services/control/src/authoring/authoring-store.ts:1001`.)*
- **The drafted scope artefacts** produced from the above. *(Anchor: `scope_docs.content`,
  `services/control/src/authoring/authoring-store.ts:1058`.)*

**None of this is discarded.** Unlike §1.4's Connect verdict commitment, the portal's job is to build
something durable from what you give it — a specification — so persistence is the point, not a gap.
The commitment in §1.4 is explicitly Connect-only and does not extend to the portal; nothing here
should be read as implying the portal checks-and-discards.

### 2.2 Why it's retained

The portal's guided authoring is resumable across sessions and re-editable — you can leave and come
back to a draft, and the product can regenerate downstream artefacts from earlier answers. That
requires keeping the answers, uploads and transcript, not just their most recent synthesis.

### 2.3 Model providers, applied to portal content

Portal content (uploads, answers, transcript) that the authoring flow sends to a model for processing
is covered by §3's provider commitment below, on the same terms as Connect's guard evaluation calls.

---

## 3. Third-party model providers and training

**No specific model provider is named in this policy.** Model choice may differ by purpose within the
product and may change over time; naming one here would make this document stale the next time it
does. Instead: content that Halfcycle sends to a third-party model provider, for either Connect guard
evaluation or portal authoring, is sent only to providers Halfcycle requires to operate under terms
that prohibit using that content to train their models. The current list of providers in use is
maintained separately from this policy — on a page that can be updated without re-issuing this
document — and is linked from the published version of this page (T-17 publishes the link).

**This is a labelled commitment, re-confirmed on an ongoing basis, not a flat guarantee stated once
and assumed to still hold.** The repository's own operational record of this posture — which provider,
what its default terms say, whether Halfcycle has a stronger zero-data-retention arrangement in place
— is explicitly kept as **a snapshot, not a standing guarantee**, re-confirmed before any real
engagement is allowed to send more than the descriptor-only data of §1.1. *(Anchor:
`docs/operations.md` — the provider-credential provisioning record and its re-confirmation
requirement.)* Whether Halfcycle is currently on a zero-data-retention arrangement with its model
provider is **not settled by this document** — it is an open operator question
(`docs/features/connect-terms-and-data-rights.md`, Open questions) — and this policy does not assume
either answer.

**Separately, and within Halfcycle's own control:** Halfcycle does not use content you submit — through
Connect or the portal — to train its own models. This is Halfcycle's own promise, distinct from the
provider commitment above, and is the one promise in this section Halfcycle alone controls.

---

## 4. Where your data is processed

This policy does not state a data-residency default, because there isn't one to state. Region is
per-engagement configuration; no public copy promises residency by default (INV-012,
`docs/invariants.md`). If a specific region matters to you, confirm your engagement's configuration
directly rather than relying on an assumption from this document.

---

## 5. Anchor table

Every present-tense factual claim above, in one place, so it can be checked against its source
independently of the surrounding prose.

| Claim | Section | Anchor |
|---|---|---|
| Guard evaluation carries no `content`, no `diff`, no file body | §1.1 | `docs/connect-wire-payloads.md` §2 |
| `sourceExcerpts` is built/reachable, off by default on the runner | §1.2 | `packages/runner/src/config.ts` (`readExcerptConfig`, `HALFCYCLE_SOURCE_EXCERPTS`) |
| Non-opted-in excerpts are stripped hosted-side before evaluation | §1.2 | `services/guard/src/routes/evaluate.ts` |
| No route can set an engagement's excerpt opt-in today | §1.2 | `services/control/src/engagements/engagement-store.ts` (`setSourceExcerptOptIn`), `services/control/src/engagements/index.ts` (sole re-export). Cited by symbol, not line — see §1.2 |
| Nothing sent that isn't descriptor data; no repo clone; no URL credentials; no cross-engagement reads | §1.3 | `docs/connect-wire-payloads.md` §6 |
| Verdict check-and-discard + training foreclosure is a decision, not shipped | §1.4 | `docs/connect-product-spec.md` §8 item 8 |
| No verdict route exists; `verdict-calls` has no writer | §1.4 | `services/control/src/entitlement/quota.ts` (`QUOTA_DIMENSIONS_AWAITING_A_WRITER`) |
| Engagement creation is counted per source network | §1.5 | `services/control/src/routes/engagements.ts` — the `recordUsage({ kind: 'source-address', key: request.ip }, ...)` call |
| The stored key is a truncated network prefix, never a full address | §1.5, §1.6 | `services/control/src/entitlement/quota.ts` — `truncateSourceAddress`, applied inside `recordUsage`; `services/control/test/account-deletion.test.ts` reads `quota_usage` back and asserts no row matches a bare address |
| Full addresses written before that shipped were deleted, not shortened | §1.5 | `services/control/src/store/migrations/1788000000000_account-deletion.sql` |
| IP-keyed quota rows carry no FK to the engagement/account | §1.5, §1.6 | `services/control/src/store/migrations/1787100000000_quota.sql` — `quota_usage`, "Deliberately ABSENT: a FOREIGN KEY from `subject_key` to `engagements`" |
| Account deletion is built and runs in one transaction | §1.6 | `services/control/src/accounts/account-deletion.ts` — `deleteAccount` |
| It reaches every table with a foreign key to an account or a project | §1.6 | `ENGAGEMENT_FK_DISPOSITIONS` / `ACCOUNT_FK_DISPOSITIONS`; `services/control/test/account-deletion.test.ts` asserts both maps against the live schema in both directions |
| **And the metering tables, which have no foreign key at all**, for the account-keyed and project-keyed rows | §1.6 | `SUBJECT_KEY_DISPOSITIONS`; `services/control/src/entitlement/quota.ts` — `deleteQuotaSubjects`. Asserted by column rather than by foreign key, because a foreign-key check is structurally blind to these three tables |
| Network-keyed metering rows are NOT reached, and cannot be | §1.5, §1.6 | `deleteQuotaSubjects` matches only the `account` and `engagement` subject kinds; there is nothing on the other end of a `source-address` key to match on |
| Deletion is operator-run; there is no self-serve delete control | §1.6 | `services/control/src/scripts/account-admin.ts`; `docs/operations.md` — "Deleting an account". No route deletes an account |
| A shared project survives, and the other person's access is untouched | §1.6 | `services/control/test/account-deletion.test.ts` — "deleting one user of a SHARED engagement strands nobody" |
| Benchmark rows are retained with the project reference removed | §1.6 | `services/control/src/accounts/account-deletion.ts` (`benchmark_rows: 'de-link'`); `1788000000000_account-deletion.sql` makes the column nullable |
| A project created before sign-in existed has no owner and cannot be reached by a deletion | §1.6 | `engagements.owner_account_id` is nullable and no code path claims one; `services/control/test/account-deletion.test.ts` — "an OWNERLESS engagement is untouched" |
| The account record holds five fields, four of them personal data, plus the ownership link | §1.8 | `services/control/src/accounts/types.ts` (`PersistedAccount`); `1787400000000_accounts-and-ownership.sql` |
| Portal retains upload full text | §2.1 | `authoring_uploads.full_text`; `services/control/src/authoring/authoring-store.ts:801,845` |
| Portal retains typed answers | §2.1 | `authoring_answers.answer_text`; `services/control/src/authoring/authoring-store.ts:59` |
| Portal retains the full transcript | §2.1 | `authoring_transcript.text`; `services/control/src/authoring/authoring-store.ts:1001` |
| Portal retains drafted scope artefacts | §2.1 | `scope_docs.content`; `services/control/src/authoring/authoring-store.ts:1058` |
| Provider non-training posture is a re-confirmed snapshot, not a standing guarantee | §3 | `docs/operations.md` — provider-credential provisioning record |
| No watermarking, no anomaly detection exists | (context for §1, not a claim made in this policy) | `docs/features/delivery-service.md:429` |

---

## Non-goals

- Cookie banners, marketing-site compliance, a DPA, sub-processor list, SOC 2, or per-jurisdiction
  variants — out of scope for this phase.
- Naming a specific model provider (§3).
- Describing portal retention as time-bounded or as a checked-and-discarded flow — it is neither; §2.1
  states plainly that it persists.

## Version history

| Version | Date | Note |
|---|---|---|
| `1.0.1` | 2026-08-19 | **T-17, post-approval fills, not a legal re-review — recorded as its own version because a version string must map to one text (T-18 records acceptance against it).** Two changes, both filling a blank the reviewed `1.0.0` text already carried, neither adding a new obligation: (1) §1.2's drafting-process narration ("earlier drafting of this policy said... that is not accurate") is removed; the corrected two-halves explanation directly below it, which `readme-claims.test.ts` pins, is unchanged. (2) §1.6's contact route — "set at publication alongside the effective date above", unfilled in `1.0.0` — is now `hello@halfcycle.ai`, the same address the terms of service publish for TOS-DIS-1. **In force as of 2026-08-19** — hosted at `Halfcycle-AI/halfcycle-cli` (`docs/legal/privacy-policy.md`, T-26); no text below the header changed to publish it. |
| `1.0.0` | 2026-08-19 | Legal review complete (T-15): approved as it stands, no edits owed. The `-draft` qualifier and the "requires a lawyer's pass" status are dropped; no substantive text changed between `0.2.0-draft` and this version, so an acceptance recorded against either is an acceptance of the same document. Still not in force — the effective date is set at publication (T-17). |
| `0.2.0-draft` | 2026-08-18 | Account deletion shipped (T-19), so §1.6 is rewritten in the present tense and its deferred IP question is settled: the quota key is truncated to a network prefix before it is stored. §1.5 rewritten to match. New §1.8 enumerates the account record's four pieces of personal data and the ownership link, which §1 previously did not mention at all. The `benchmark_rows` retention is now stated here, as the decision required. Two code anchors re-cited by symbol rather than by line number, after three different line numbers were recorded for one unchanged fact inside a week. Still not published; still pending legal review. |
| `0.1.0-draft` | 2026-08-17 | Initial substance draft (T-14). Not published. Pending legal review. |
