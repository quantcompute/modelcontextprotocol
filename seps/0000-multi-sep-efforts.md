# SEP-{NUMBER}: Epic records for multi-SEP efforts

- **Status**: Draft
- **Type**: Process
- **Created**: 2026-10-13
- **Author(s)**: Kryspin Ziemski <krys&#64;qcomp.io> (@kziemski)
- **Sponsor**: None
- **PR**: https://github.com/modelcontextprotocol/modelcontextprotocol/pull/{NUMBER}

## Abstract

This SEP defines an **Epic**: a durable, citable record of one outcome that requires more than one SEP. An Epic states the outcome, its non-goals, the SEPs that together deliver
it, the dependencies between those SEPs, a coordinator drawn from existing authority, and an observable done criterion. It carries four effort states of its own, and no authority
over any individual SEP.

This SEP standardizes the capability, not the mechanism: it adds no new SEP status, label, or preamble field of its own, and does not decide the mechanism that carries Epics
([Deferred mechanism](#deferred-mechanism)). No individual SEP's lifecycle changes.

## Motivation

MCP tells authors to decompose large work and then provides no artifact that holds the resulting set: "Consider whether your change might be too large - could it be split into
smaller SEPs?" (`docs/community/contributing.mdx:564`).

- **The roadmap's cross-WG work has no record.** The roadmap asks for a composition review across the Agents, Transports, and Triggers & Events working groups and for an extension
  contract owned by the SDK WG with the Core Maintainers (`docs/development/roadmap.mdx` § "Priority Areas").
- **Tasks spans two SEPs and a planned core promotion.** SEP-1686 (`seps/1686-tasks.md` preamble; Standards Track, Final, 2025-10-20) plus SEP-2663 (`seps/2663-tasks-extension.md` preamble;
  Extensions Track, Final, 2026-04-27; "on behalf of the Agents Working Group"), with promotion to core planned (`docs/development/roadmap.mdx` § "Priority Areas"). No artifact
  says the two form one effort or what done means for the pair, and the `tracking` issue covering SEP-2663, #2886, is CLOSED while work continues.
- **The pre-agreement container is excluded by design.** The work-tracking form covers work "already been discussed and agreed on — not a new proposal"
  (`.github/ISSUE_TEMPLATE/work_tracking.yml:2-4`), and is used flat: 53 `tracking` issues exist, 19 of them one-per-SEP "(implementation): SEP-NNNN" trackers, none with a git
  sub-issue.

**Lived consequence, not proof of causation.** SEP-1300 (a tool-filtering primitive) and [SEP-2084](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2084) (a
server `Groups` primitive) were protocol-surface proposals rejected on pattern-immaturity grounds: "it's unclear what mechanism is the right one in the longrun… not too early
standardize a pattern while the patterns in the de-facto implementations are still evolving"
([2026-02-05](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2084#issuecomment-3854288669), tally "🟢 0 Accept, 🟡 0 Accept with changes, 🔴 6 rejects" in
[Discussion #2204](https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/2204)). That the effort was tracked as three numbered proposals, with no shared record
of membership, boundary, or done criteria, is a **separate observation** about how it was tracked, and is **not offered as the cause of the rejection**. The effort ran four months,
and a participant disputed whether three people could speak for the group ("if 3 people can't agree then why listen to the community at all?" —
[scottslewis](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2084#discussion_r2718702402)), while an author wrote that "prior effort with SEPs 1300 and 993
basically exhausted everyone on the topic" ([cliffhall](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2084#discussion_r2718661827)). A comparable pattern
appears in a later attempt to consolidate two working groups, which was abandoned
([#3389](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/3389#issuecomment-6105433582)).

**What this SEP does not propose.** Early alignment already shipped: issue [#1435](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1435) asked to let authors
"ask for alignment before committing to writing a full SEP", and the 2026-09-30 meeting recorded "Pre-SEP direction requests use the existing queue with a written artifact"
(https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3407). Nor does this SEP propose a new SEP status — an earlier attempt to add states to the SEP machinery
left no trace in today's status table (`docs/community/sep-guidelines.mdx` § "SEP Statuses") — or more working-group autonomy
([#1434](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1434)); it exists because working groups "have more ideas than actionable work and need explicit
off-ramps" (https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3403). **Design stance:** this SEP follows the SEP-2084 rejection stance rather than arguing
with it, deferring the mechanism ([Deferred mechanism](#deferred-mechanism)) instead of standardizing a pattern that is still evolving.

## Specification

### Scope

This SEP governs the semantics of a record that states what a set of SEPs together deliver. It does not govern the lifecycle, status, numbering, sponsorship, review, or approval of
any individual SEP, and it does not govern working-group structure, the roadmap, or the surface on which agreed work is executed. This SEP does not change the protocol, the schema,
or any wire behaviour; the construct is a process record only.

An **effort** is a body of work a group intends to complete. An **Epic** is a durable, citable record of one effort requiring more than one SEP. A **member SEP** is a SEP an Epic
names as part of itself, and a **Coordinator** is the one person accountable for an Epic.

### The Epic record

1. **Outcome.** Every Epic MUST state the single outcome its member SEPs together deliver.
2. **Scope boundary.** Every Epic MUST state its non-goals explicitly, in addition to its goals.
3. **Membership.** Every Epic MUST name the SEPs that constitute it; each member SEP remains independently sponsored, reviewed, and approved.
4. **Dependencies and ordering.** Every Epic MUST state which member SEPs constrain which, and the consequence for the effort if a member SEP is rejected or withdrawn.
5. **Coordinator.** Every Epic MUST name exactly one Coordinator, who MUST already hold authority in the affected area as a Working Group Lead or a Maintainer. This SEP creates no
   role.
6. **Done criteria.** Every Epic MUST state the observable condition under which the effort is complete, and the Coordinator MUST mark it complete when that condition holds.
7. **Lifecycle.** Every Epic MUST be in exactly one of four states — proposed, active, complete, abandoned — and the Coordinator MUST record a reason on abandonment. These are
   states of the effort; they MUST NOT add, rename, or reorder SEP statuses.
8. **Non-authority.** An Epic MUST NOT gate, rank, delay, or block any individual SEP, and MUST NOT merge or dissolve a Working Group. Precedence, stated explicitly and in this
   order: **roadmap > working-group roadmap > Epic > individual SEP**.
9. **Boundedness.** Every Epic MUST span two or more member SEPs. If an effort reduces to a single SEP, or membership otherwise falls below two, the Coordinator MUST withdraw the
   Epic and that SEP proceeds alone.
10. **Discoverability without mechanism.** Every Epic MUST carry a stable identifier unique within the project's SEP system, and each member SEP MUST be reachable from its Epic and
    the Epic from each member SEP. Neither direction's location or form is specified by this SEP; see [Deferred mechanism](#deferred-mechanism).

An author MAY maintain a record of an effort that is never declared as an Epic; such a record is allowed but unsupported.

**Volume, cost, and dormancy.** An Epic MUST be declared by, and carry the agreement of, the working group that owns the area; the declaring SEP's PR description links that
discussion, inheriting the gate the process already requires (`docs/community/sep-guidelines.mdx` § "Step-by-Step Process"). An Epic adds no scheduled review item, no periodic
report, no status meeting, and no per-member review: it is reviewed once, with its own SEP, at the existing biweekly meeting (`docs/community/sep-guidelines.mdx` § "SEP Review &
Resolution"), consistent with the repo's demonstrated preference against standing reports
([Discussion #3402](https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3402) §6). The Coordinator MUST consent; a dispute over an Epic goes to the Core
Maintainers. Any listing of Epics MUST be generated from the records rather than hand-maintained, and a retired Epic is not deleted while its member SEPs remain
(`seps/2149-working-group-charter-template.md` § "1.9 Lifecycle"). The six-month sponsor clock and automated `dormant` handling apply to proposals awaiting a sponsor
(`docs/community/sep-guidelines.mdx` § "SEP Statuses"); an Epic is declared with the agreement of its working group and a Coordinator, so it sits in the sponsor-backed stream from
the start, and the automation that closes dormant proposals does not reach it.

### Adoption path

Non-normative. This SEP standardizes the **capability**, not a carrier: the carrier is chosen only after the
capability has been used, in a follow-up this SEP does not file on its own. The initial use is composed entirely of material the repo already requires: a charter's in-scope / out-of-scope and success criteria, a `tracking` issue's
agreement link and done criteria, and the SEPs themselves. What is new is requiring them **together, in one citable record, with a done criterion** — not the invention of new
fields. Pilot: the Tasks record ([Reference Implementation](#reference-implementation)) plus one forward-looking in-flight effort, the Transports WG's multi-SEP programme
(`docs/community/working-groups/transports.mdx` § "Mission Statement"). If the pilot shows no use, the capability is dropped; the mechanism SEP is not filed. Adopting a proven
pattern from elsewhere is in scope for this project: the charter template "was adapted from Kubernetes governance structures"
(`seps/2149-working-group-charter-template.md:387-389`).

### Roles

| Action                                                                                                                | Who                                                                                                                    |
| --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Declare an Epic and name its Coordinator, and record its outcome, non-goals, members, dependencies, and done criteria | Any contributor, with the owning working group's agreement and a Coordinator who is a Working Group Lead or Maintainer |
| Move an Epic to active, complete, or abandoned                                                                        | The Coordinator                                                                                                        |
| Sponsor, review, and approve a member SEP                                                                             | Per the SEP process; unchanged by this SEP                                                                             |
| Approve a member SEP                                                                                                  | Core Maintainers, per the [governance decision process](/community/governance#decision-process)                        |

### Cross-record effects

An Epic touches five records, and this SEP changes none of them; each owner is named below.

| Record                                 | Owner                                                                                                                                  | Changed |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| The SEP record and its status          | The SEP's sponsor (`docs/community/sep-guidelines.mdx` § "Status Management")                                                          | No      |
| The generated SEP index and navigation | The SEP rendering tooling (`scripts/render-seps.ts`)                                                                                   | No      |
| Working-group charters                 | Working Group Leads (`seps/2149-working-group-charter-template.md:400`, "Leads/Facilitators are responsible for drafting the charter") | No      |
| The roadmap                            | Core Maintainers (`docs/development/roadmap.mdx` § "SEP Prioritization")                                                               | No      |
| Agreed work awaiting execution         | The `tracking` process (`.github/ISSUE_TEMPLATE/work_tracking.yml`, `labels: [tracking]`)                                              | No      |

`tracking` stays the execution surface for agreed work; an Epic is the agreement-time record.

### Deferred mechanism

Non-normative. This SEP does not choose how an Epic is carried, and none of the following is a decision or a default: (a) a value in the SEP preamble's type field; (b) an index
file for Epics plus a generated page; (c) a work-tracking-labelled issue using the title convention used elsewhere; (d) a column in the generated SEP index; (e) a section in each
working-group charter. Naming is deferred too: this document uses **Epic**, and the alternatives considered once are "umbrella", "programme", and "tracked effort". This mirrors
[SEP-2133](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2133) naming what it omits because those parts "may be added later"
(`seps/2133-extensions.md:223-231`).

## Rationale

**The vocabulary exists; the record does not.** [SEP-1302](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1302) states a Working Group's goal as "facilitate
MCP community collaboration on a specific SEP, themed series of SEPs, or officially endorsed Project" (`seps/1302-formalize-working-groups-and-interest-groups-in-mc.md:84`),
repeated in the published page (`docs/community/working-interest-groups.mdx` § "Working Groups (WGs)").

**Rivals, conceded.** A working group already coordinates multi-SEP work: its charter carries in-scope, out-of-scope, related groups, and active work items
(`docs/community/charter-template.mdx` § "Scope", § "Deliverables & Success Metrics (WG only)"), and a live charter points at its upstream repository for "proposal strategy"
(`docs/community/working-groups/transports.mdx:16-19`). A single SEP can itself be a chartering document (`seps/2127-mcp-server-cards.md`, the chartering note), and the
maintainers' remedy for scale is more working-group autonomy, not a new record type (https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1434). If an effort has one
outcome and one implementation, a single SEP is correct and this SEP does not apply to it: an Epic requires two or more member SEPs and is withdrawn if it reduces to one. Autonomy
plus a charter cannot carry the cross-WG case, because a charter is per-group and a roadmap rewrite does not preserve it (`docs/development/roadmap.mdx`), and the roadmap itself
asks for a cross-WG composition review (`docs/development/roadmap.mdx` § "Priority Areas"). The charter template's own failure mode is group-level: "**Scope Creep**: Without
explicit boundaries, groups may gradually expand into areas owned by other groups or outside their mandate" (`seps/2149-working-group-charter-template.md:26`).

**The narrowed gap, and prior art.** For a bounded, cross-cutting effort spanning more than one working group, no artifact carries boundaries and non-goals, the member set,
inter-SEP dependency, done criteria, and durability past a roadmap rewrite (`docs/development/roadmap.mdx`). Authors are told to split large proposals and given no container
(`docs/community/contributing.mdx:564`), and the roadmap's composition review and extension contract span three and two groups with no per-effort record
(`docs/development/roadmap.mdx` § "Priority Areas"). The written artifact [Discussion #3407](https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3407)
established for pre-SEP direction — "a doc, a PR, or a decision record in the WG repo" — is free-form and per-direction; it carries no membership, no cross-SEP dependency and no
done criterion, and nothing links it to the set of SEPs that later deliver the outcome. That is the delta, and no more than that. Searches for `epic`, `umbrella`, `super SEP`,
`parent SEP`, `SEP grouping`, `program of work`, `too many SEPs`, and `review fatigue` return no on-topic
hits in this repository's issues, pull requests, or discussions.

**What another project's epics show, and do not show.** A2A uses exactly 8 epics, where "epic" is a bare title convention — no label, no issue type, no template, no documentation
mentioning epics, and no native sub-issues (https://github.com/a2aproject/A2A/issues/1986, https://github.com/a2aproject/A2A/issues/1995). All 8 remain open, so an epic there
cannot distinguish "shipped" from "abandoned"; the set originated in a backlog-consolidation vote (https://github.com/a2aproject/A2A/issues/1942) that approved five umbrellas,
after which execution created seven, and an eighth followed (#1995). Read as demand evidence, this supports one narrow claim — a named record over already-discussed work with an
explicit done state is useful — and the uncodified convention is what let the eighth epic and divergent shapes appear. MCP's durable SEP numbers
(`docs/community/sep-guidelines.mdx` § "Step-by-Step Process"), `final` and `superseded` statuses preserved as historical records ("They are not updated after finalization",
`docs/community/sep-guidelines.mdx:260`), and single-SEP conformance traceability (`docs/community/sep-guidelines.mdx` § "Conformance Test Requirement") must not be displaced.

**Alternatives considered.** _Add a SEP status._ Rejected: an earlier attempt to add states left no trace in the current status table (`docs/community/sep-guidelines.mdx` § "SEP
Statuses"). _Add a new SEP type._ Deferred, not rejected: it is a mechanism candidate ([Deferred mechanism](#deferred-mechanism)), and adding a value is a template change
(`seps/TEMPLATE.md` § "Preamble").

**Non-blockers, and concurrent work.** No CI gate validates the SEP type — `check:seps` is only `tsx scripts/render-seps.ts --check` (`package.json` § `scripts`) — and the renderer
reads the type as a plain column, so a new value renders without code changes. [SEP-3392](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/3392) and
[SEP-3398](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/3398) (release cycle, shared review queue, pre-review section) are both open and unmerged as of
2026-10-13, and
[#3402](https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3402)/[#3407](https://github.com/modelcontextprotocol/modelcontextprotocol/discussions/3407) record
them as heading to a vote; this SEP is compatible with both, requires no change to SEP-3398 or SEP-3392, and an Epic must be expressible within whatever intake artifact SEP-3398
lands. SEP-3392 classifies a different object, and SEP-3398 schedules review. Maintainer review time is scarce (`docs/development/roadmap.mdx:18`), and where an effort already sits
inside one working group's charter, the answer is the charter, not an Epic.

**Recording.** When adopted, affected pages carry a back-reference of the form used by SEP-2596 — "This policy was adopted via
[SEP-2596](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2596)." (`docs/community/feature-lifecycle.mdx:12-13`).

## Backward Compatibility

This SEP introduces a process and does not change protocol behavior. It is **not retroactive**: SEPs that reached Final before it takes effect are unaffected, no existing SEP is
required to declare an Epic, and no existing SEP's status, number, or content changes. SEP-1686 and SEP-2663 remain Final records; the Epic record in
[Reference Implementation](#reference-implementation) is added alongside them and governs nothing in them.

The change lands in the documents named below, non-normatively: `docs/community/sep-guidelines.mdx` (types and statuses), `seps/TEMPLATE.md` (preamble),
`docs/community/feature-lifecycle.mdx`, `docs/community/working-interest-groups.mdx`, `docs/development/roadmap.mdx` (the precedence statement), and the generated
`docs/seps/index.mdx` via `scripts/render-seps.ts`. Following the counter-example of SEP-2085, whose promised insertion into `docs/community/governance.mdx` never landed
(`seps/2085-governance-succession-and-amendment.md` § "Reference Implementation"), the doc edits land in this SEP's own pull request. The PR must run `npm run generate:seps` and
commit the generated `docs/seps/<number>-multi-sep-efforts.mdx`, `docs/seps/index.mdx`, and `docs/docs.json`, and the process makes the rename to the PR number mandatory before
merge (`docs/community/sep-guidelines.mdx` § "Step-by-Step Process"). `check:seps` already fails at current HEAD on pre-existing SEP-2127 drift — the missing `/seps/2127` redirect
and the `Final` count 42→43 — so a red check in this PR may not be ours.

## Security Implications

None: this SEP adds no message, capability, transport, schema type, or authentication requirement, and collects no data. Its one security-relevant property is the non-authority
clause.

## Reference Implementation

This SEP defines a process and has no runnable reference implementation. Its reference implementation is the artifacts below; these are this SEP's own reference implementation
checklist, not ongoing process requirements (compare `seps/2484-conformance-tests-required-for-final-seps.md` § "Prerequisites for Final status").

For Process SEPs the prototype criterion (`docs/community/sep-guidelines.mdx` § "SEP Review & Resolution"; "a prototype implementation demonstrating the proposal",
`docs/community/sep-guidelines.mdx:162`) has been satisfied by documentation-only artifacts in every accepted case — `seps/1850-pr-based-sep-workflow.md` § "Reference
Implementation", `seps/2149-working-group-charter-template.md` § "Reference Implementation", and `seps/2596-spec-feature-lifecycle-and-deprecation.md:332` ("This SEP defines a
process and has no reference implementation") — and this SEP's enumerable artifact is the filled Tasks record below.

1. **The filled Epic record for the Tasks effort**, recorded from artifacts that already exist: the two Final SEPs (`seps/1686-tasks.md` preamble, `seps/2663-tasks-extension.md`
   preamble) and the roadmap's staging statement (`docs/development/roadmap.mdx` § "Priority Areas").
2. **One back-pointer from each member SEP** to that record, and the reverse member list in the record.
3. **The documentation edits that carry this policy**, in this SEP's own pull request (`docs/community/sep-guidelines.mdx` § "SEP Review & Resolution").

The table renders the capability in [Specification](#specification) against the Tasks effort. It is an illustrative rendering, **not a proposed template**, and its field headings
are non-normative.

| Field         | Content for the Tasks effort                                                                                                                                                                                                                                                                                                                                                |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Outcome       | One coherent Tasks capability available to servers and clients, culminating in the extension's inclusion in the core protocol (`docs/development/roadmap.mdx` § "Priority Areas").                                                                                                                                                                                          |
| Non-goals     | Not the server-initiated event surface itself, and not the composition review spanning Agents, Transports, and Triggers & Events (`docs/development/roadmap.mdx` § "Priority Areas").                                                                                                                                                                                       |
| Members       | SEP-1686 (`seps/1686-tasks.md` preamble) and SEP-2663 (`seps/2663-tasks-extension.md` preamble).                                                                                                                                                                                                                                                                            |
| Dependencies  | SEP-1686 is preserved as a historical record whose feature moved into the extension (`seps/1686-tasks.md:11`, "preserved as a historical record"); promotion to core depends on the extension remaining stable — "Once the extension has stabilized and achieved broad adoption, it is intended to be promoted into the core protocol" (`seps/2663-tasks-extension.md:15`). |
| Coordinator   | The Agents Working Group Lead — the existing authority SEP-2663 was filed on behalf of (`seps/2663-tasks-extension.md` preamble).                                                                                                                                                                                                                                           |
| Done criteria | No artifact in this repository states a completion condition for the Tasks effort; recording it as an Epic is what would require one to be written. The nearest statement of intent (`docs/development/roadmap.mdx` § "Priority Areas") is a direction, not a done criterion.                                                                                               |

---

## Open Questions

- **The prototype carve-out.** For Process SEPs the prototype criterion (`docs/community/sep-guidelines.mdx` § "SEP Review & Resolution", § "Prototype Requirements") has been
  satisfied by documentation-only artifacts in every accepted case, so the sponsor is asked to confirm that reading, or to extend the exemption in § "Conformance Test Requirement"
  to prototypes for Process SEPs.
- **Where an Epic is carried.** Deferred deliberately; see [Deferred mechanism](#deferred-mechanism).
- **Whether an Epic needs acknowledgement.** This SEP adds no acknowledgement requirement beyond the prior discussion the process already requires
  (`docs/community/sep-guidelines.mdx` § "Step-by-Step Process").
