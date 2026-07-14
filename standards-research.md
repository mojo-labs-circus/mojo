# How real standards actually got made

Research findings, 2026-07-13/14. This file answers the question underneath the
whole project: how have standards actually been made, end to end, historically.
Not just how the piece/seam decomposition gets found, but how it turns into a
written document, how that document gets validated, when and in what state it
goes public, and how it attracts people afterward.

This is a research archive, same standing as research-plan.md: findings with
sources, checked against real history, not decisions. What Mojo actually does
with this lands in roadmap.md and the standard-shaping document (AGENTS.md
Current stage, item 2) only after being talked through. Unverified items are
flagged inline and collected at the end.

Checked against: POSIX, Kubernetes (CRI, CNI, CSI, admission webhooks), LSP,
OCI, Matrix, XMPP, Linux, IETF/W3C process, and the failure cases (OSI, CORBA).

---

## 1. How the decompositions were actually found

Three distinct patterns exist, not one.

### POSIX: convergence, with bounded invention where convergence failed

Before state: by the early 1980s Unix had fractured into incompatible
commercial lines (System III/V, 4.2/4.3BSD, Xenix, dozens of vendor variants).
Application vendors could not write one portable program. The /usr/group
association (a user/vendor trade group, not AT&T) published the /usr/group 1984
Standard, the first written convergence attempt; IEEE P1003 formed 1984-85
building directly on it, trial-use standard 1986, IEEE Std 1003.1-1988. The
documented basis: "largely based on UNIX Seventh Edition, UNIX System III,
UNIX System V, 4.2BSD and 4.3BSD documentation." The standard was written by
diffing real shipping systems. The forcing function was US government
procurement: NBS/NIST turned POSIX into FIPS 151, making conformance a
purchasing requirement.

POSIX was not purely convergent. Where real systems had irreconcilable
observable semantics under the same name, the committee invented a new surface.
Three verified cases:

- termios/tcgetattr/tcsetattr: System V used a termio struct via ioctl, BSD
  used sgtty via ioctl. ioctl itself was unstandardizable (variadic, untyped),
  so POSIX.1-1990 standardized modified SysV semantics under new function
  names. Semantics converged, call surface invented.
  <https://man7.org/linux/man-pages/man7/termio.7.html>
- O_NONBLOCK: SysV's O_NDELAY returned 0 on no data (indistinguishable from
  EOF), BSD's returned -1/EWOULDBLOCK. Same name, incompatible behavior,
  unarbitratable. POSIX invented a third flag with -1/EAGAIN semantics.
  <https://mail.python.org/pipermail/python-list/1999-May/013687.html>
- pax: the tar vs cpio war could not be reconciled, so P1003.2 designed a new
  utility supporting both formats. Ratified 1992, and the utility never
  displaced tar in practice, a standing caution that committee inventions can
  be ratified yet ignored. The pax interchange format did land in later tar.
  <https://en.wikipedia.org/wiki/Pax_(command)>

The rule: existing practice first, invent only to break ties, never
speculatively. The invention always sat at the exact seam where diffing failed.

### Kubernetes: three different patterns inside one project

**CRI: extraction, triggered by a second implementation arriving.** Docker
support was compiled directly into the kubelet through "an internal and
volatile interface." The trigger was rkt: the rktnetes effort (K8s 1.3,
mid-2016) tried to add rkt the same hardcoded way and the pain forced the
interface out. CRI shipped alpha in 1.5 (Dec 2016), gRPC, two services, the
pod-sandbox abstraction shaped by what the kubelet already did against Docker.
One implementation means no interface needed; two means extraction.
<https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes/>

**CNI: extraction from rkt's design, then a market contest.** CNI originated
at CoreOS, derived from the rkt Networking Proposal, generalized into a
runtime-neutral spec (JSON config plus an exec'd binary with ADD/DEL verbs).
Developed in parallel with a competitor, Docker's libnetwork/CNM. Kubernetes
evaluated both and picked CNI; it then became shared across rkt, K8s, Mesos,
Cloud Foundry. Extraction from one system, then two extracted interfaces
competing, settled by a big adopter choosing one.
<https://github.com/containernetworking/cni/blob/spec-v0.3.1/SPEC.md>
<https://thenewstack.io/container-networking-landscape-cni-coreos-cnm-docker/>

**CSI: years of in-tree pain, then a multi-orchestrator negotiated spec.**
Vendor volume drivers compiled into core K8s locked vendors to the release
cadence and put third-party code in core binaries. FlexVolume (exec-based
escape hatch) failed on its own terms. CSI was designed cooperatively across
Kubernetes, Mesos, Cloud Foundry, and Docker in a neutral repo; alpha 1.9
(Dec 2017), GA 1.13 (Dec 2018). Closest of the three to convergence, but a
convergence of needs among parties each already holding a working storage
layer, not a diff of identical shipped interfaces.
<https://github.com/kubernetes/design-proposals-archive/blob/main/storage/container-storage-interface.md>

**Admission webhooks: extraction of an extension point.** Admission
controllers were compiled-in chain links; downstream distros (OpenShift)
carried custom ones; K8s 1.7 introduced external admission webhooks so the
logic could live out-of-tree. Same shape as CRI.
<https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/>

### LSP: pure extraction, generalized from one shipped protocol

Three precursors, well documented by Microsoft's own history page: OmniSharp
(community C# server, HTTP+JSON), Microsoft's TypeScript tsserver
(stdin/stdout JSON inspired by the V8 debugger protocol), and VS Code/Monaco's
internal language API. The history page states the move directly: "after
having consumed two different language servers the VS Code team [explored] a
common language server protocol"; the protocol "takes the TypeScript server
protocol as a starting point," made general and language-neutral, plus
capability negotiation, on JSON-RPC. Published 27 June 2016 at DevNation,
jointly with Red Hat and Codenvy: published exactly when other tool vendors
wanted in, the moment one consumer became many. No convergence phase across
independent implementations before publication; independent implementations
came after.
<https://github.com/microsoft/language-server-protocol/wiki/Protocol-History>
<https://code.visualstudio.com/blogs/2016/06/27/common-language-protocol>

### OCI: extraction from Docker's shipped artifacts, under competitive pressure

Docker's format and runtime were the de facto standard by shipped volume;
CoreOS's appc spec plus rkt (Dec 2014) created a real fork risk. June 2015,
DockerCon: Docker donated the image format, runc (carved from libcontainer),
and the draft specs to a new Linux Foundation project, with appc maintainers
joining the board. runtime-spec was written around already-shipping runc;
image-spec (April 2016) started from the Docker v2.2 format, v1.0.0 July 2017;
distribution-spec later, extracted from the Docker Registry HTTP API. Every
OCI spec is a post-hoc write-down of one shipped implementation; the trigger
for writing was the existence of a competing spec.
<https://opencontainers.org/posts/announcements/2015-06-20-industry-leaders-unite-to-create-project-for-open-container-standard/>
<https://lwn.net/Articles/649194/>

### Matrix: a-priori design, the outlier, and it paid for it

Started 2014 inside Amdocs by a team that had shipped the previous generation
of telecom messaging and concluded SIP/SMPP/XMPP "were not built for the web."
The protocol (replicated room DAG, state resolution, federation over
HTTPS+JSON) was designed from scratch: pain-informed invention, not
convergence, not extraction. It then spent years revising itself (state
resolution v2, twelve room versions) because a-priori federation design got
things wrong that only production surfaced. Their own stated method going in:
"build an implementation first... and then use that implementation to prove
that the protocol works. Then, having implemented it, we would spec it."
<https://matrix.org/docs/older/faq/>
<https://lwn.net/Articles/1009932/>

XMPP, the contrast case, was textbook extraction: jabberd server early 1998,
protocol was whatever jabberd spoke through 1999-2000, handed to an IETF
working group only in 2002, RFCs in October 2004. Six years of running code
before ratification. <https://xmpp.org/about/history/>

### Cross-cutting

- Pure convergence: POSIX only, and even POSIX invented at every seam where
  diffing failed.
- Extraction is the dominant modern pattern (CRI, CNI, LSP, OCI, admission
  webhooks, XMPP). The publication trigger is remarkably consistent: the
  arrival of a concrete second party (rkt for CRI, Red Hat/Codenvy for LSP,
  appc for OCI, downstream distros for admission webhooks). Interfaces get
  pulled out of working systems when someone real is standing at the door.
- A-priori design is rare and expensive: Matrix, done by veterans, paid in
  years of spec revision.

---

## 2. The six-problems framework, checked against prior art

seam-method.md's six problems (shape, vocabulary, discovery, versioning,
trust, liveness) checked against established frameworks.

**The Eight Fallacies of Distributed Computing** (Bill Joy/Tom Lyon originated
four, L. Peter Deutsch consolidated seven at Sun 1994, James Gosling added the
eighth): network reliable, latency zero, bandwidth infinite, network secure,
topology static, one administrator, transport free, network homogeneous.
<https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing>

Mapping: fallacy 1 maps to liveness (partially), 4 to trust (partially), 5 to
discovery, 6 to versioning plus trust, 8 to shape plus versioning. The
fallacies are about the channel; the six are mostly about the contract. They
are complementary, not competing, which is itself a validation: the six are
not a rediscovery of the fallacies, and nothing in the six is fabricated
relative to prior art.

Three candidate gaps the check surfaced:

1. **Performance physics** (fallacies 2, 3, 7): latency, bandwidth,
   chattiness. Boundary specs that ignore these produce chatty interfaces,
   the classic distributed-objects failure (CORBA/DCOM pretending remote is
   local). Real standards encode this structurally (CRI chose gRPC and
   pod-level granularity partly to bound round trips).
2. **Partial failure as distinct from liveness**: not "the other side may be
   down" but "the call may fail midway and you may never learn whether it
   ran." Indeterminacy is what forces idempotency and retry semantics into
   contracts, and it is not derivable from the other five.
3. **Independent administration** (fallacy 6) as possibly first-class: it is
   the root cause of versioning skew, of trust, and of half of discovery.
   The fallacies treat it as its own fact; the six spread it across two.

**End-to-End Arguments in System Design** (Saltzer, Reed, Clark; ICDCS 1981,
ACM TOCS 1984): a function like reliability or encryption can only be
completely implemented with the knowledge of the application at the endpoints;
lower-layer implementations are at best optimizations. It answers a different
question than the six: not what breaks at a boundary, but which piece owns a
guarantee. Directly relevant to seam design: an argument for thin seams and
smart pieces, the ancestor of the narrow waist.
<http://web.mit.edu/Saltzer/www/publications/endtoend/endtoend.pdf>

**Parnas 1972**, "On the Criteria To Be Used in Decomposing Systems into
Modules" (CACM 15(12)): module boundaries drawn by information hiding; each
module hides one design decision likely to change. The classical answer to
where a seam should land: does each seam hide exactly one axis that varies
independently (which runtime, which storage vendor, which language server)?
CRI, CNI, CSI, and LSP all pass this test explicitly.

**The Hourglass Theorem** (Micah Beck, "On the Hourglass Model," CACM 62(7),
2019): a weaker, smaller spec supports fewer applications but admits many more
implementations beneath it. Every operation added to a seam shrinks the set of
systems that can implement it.
<https://cacm.acm.org/research/on-the-hourglass-model/>

**Hyrum's Law** (<https://www.hyrumslaw.com/>): with enough users, all
observable behaviors get depended on; the de facto interface is the observed
behavior, not the spec. This is precisely what happened to LSP (section 4).

**Gall's Law** (Systemantics, 1975): a complex system that works is invariably
found to have evolved from a simple system that worked. Section 1's extraction
pattern stated as a law.

**Conway 1968** ("How Do Committees Invent?", Datamation): systems mirror the
communication structure of the organizations that build them. Descriptively
confirmed by section 1: every extraction event was triggered by a second
organization arriving. A seam worth standardizing is one where two
independently-governed parties actually meet.

---

## 3. How the document actually gets written

### Normative language

RFC 2119 (1997, BCP 14) defines MUST/SHOULD/MAY and, as important, the
discipline: imperatives used sparingly, "only where it is actually required
for interoperation or to limit behavior which has potential for causing harm,"
never to impose implementation methods. RFC 8174 (2017) had to patch a
20-year ambiguity (only ALL-CAPS is normative), a lesson that the keyword
system itself needed versioning. <https://www.rfc-editor.org/rfc/rfc2119>

POSIX uses shall/should/may/can (Base Definitions, Word Usage) and adds a
layer RFC 2119 lacks, a graded vocabulary for holes in the spec: **undefined**
(invalid input, anything goes), **unspecified** (valid input, implementation
need not document), **implementation-defined** (implementation chooses and
shall document). Deliberate gaps become first-class named objects. One of
POSIX's most-copied inventions (C uses the same triad).
<https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap01.html>

Matrix and OCI adopt RFC 2119. LSP does not: informal must/should, no
conformance clause at all. The most widely adopted of the five has the least
formal discipline, and section 4 documents the bill for it.

### Rationale

POSIX ships a separate informative Rationale volume mirroring the normative
volumes chapter by chapter, including rationale for removed interfaces, with
changes citing Austin Group defect report numbers (a traceability chain from
normative text to the bug tracker). Verified in the weak sense (rejected
alternatives, historical notes); not verified that it records formal ballot
tallies. <https://pubs.opengroup.org/onlinepubs/9699919799/xrat/contents.html>

Matrix has the most modern mechanism, the MSC process: every change is a
proposal PR, the PR number is the MSC ID for life, deliberation is public on
the PR, Final Comment Period needs 75% agreement, and **proposers must
demonstrate a working implementation proving feasibility before merge**.
<https://spec.matrix.org/proposals/>

IETF: no rationale volume; rationale lives in public mailing-list archives and
occasional design-considerations appendices. LSP and OCI: GitHub issues as
uncurated de facto rationale; interpretation disputes get settled after the
fact in the tracker.

### Document structure

- POSIX.1: four volumes. Base Definitions (shared terms, headers, formats),
  System Interfaces, Shell & Utilities, Rationale (informative). The shared
  data-format layer is factored out once and referenced by both interface
  volumes. Options are margin codes on each interface (OB obsolescent, XSI,
  CX, named option groups), one document with an option lattice.
- OCI: three specs in three repos, split by artifact lifecycle stage
  (runtime-spec, image-spec, distribution-spec), versioned independently,
  different implementer populations.
- LSP: one big markdown spec per minor version plus, since 3.17, a
  machine-readable meta-model (metaModel.json) that code generators consume,
  de facto more binding than the prose.
- Matrix: split by implementer role (Client-Server, Server-Server/Federation,
  Application Services, Identity, Push Gateway), plus Room Versions as a
  separately versioned annex. A client author needs only the C-S API.

### Conformance

- POSIX: real certification infrastructure. FIPS 151-2 (1993) made conformance
  a US federal procurement requirement, tested by NIST's PCTS; The Open Group
  took over certification in 1998. <https://www.opengroup.org/testing/fips/>
- OCI: executable conformance suite for distribution-spec (pull, push,
  discovery, management). The charter is explicit: the spec and test suites
  judge compliance, not the reference implementation; a runtime can comply
  while differing substantially from runc.
  <https://github.com/opencontainers/tob/blob/main/CHARTER.md>
- LSP: verified, there is no conformance suite, no certification, no
  conformance clause. "Conformant" has no defined meaning; in practice it
  means "works with VS Code."
- Matrix: Sytest and Complement test homeservers against the spec; gating CI,
  not a certification brand. <https://github.com/matrix-org/complement>

### Versioning and evolution

Nobody does flag-day breaking changes:

- POSIX: revisions 1988, 1990, 1996, 2001 (the Austin Group merge: one
  document adopted by IEEE, Open Group, and ISO simultaneously), 2008, 2017
  corrigenda, 2024. Interfaces are marked obsolescent (OB) one full issue
  before removal, decade-scale warning. Between revisions, public defect
  reports drive Technical Corrigenda.
- IETF: RFCs are immutable; evolution by publishing a new RFC that obsoletes
  the old. HTTP chain: 2068 (1997), 2616 (1999), 7230-7235 (2014), 9110-9112
  (2022). <https://www.rfc-editor.org/info/rfc9110>
- LSP: 3.x minor versions, features tagged @since, capability negotiation at
  initialize, and a hard rule that receivers ignore unknown values. 3.x has
  never broken the wire protocol; breakage is simply avoided.
- OCI: semver with procedural gates: major releases MUST have at least three
  release candidates a week apart; pre-1.0 specs SHOULD release monthly.
  <https://github.com/opencontainers/runtime-spec/blob/main/RELEASES.md>
- Matrix: three orthogonal axes. Global spec versions (v1.1, v1.2) replaced
  per-API versioning; every endpoint carries its own path version; room
  versions scope federation-critical breaking changes per-room, opt-in, never
  flag-day. <https://spec.matrix.org/latest/>

---

## 4. Validation: what "done" actually means

### The IETF gate, exactly

RFC 2026 section 4.1.2 (1996): advancing to Draft Standard required "at least
two independent and interoperable implementations from different code bases,"
and the requirement applied "to all of the options and features," per feature,
with unimplemented features removed before advancement. RFC 6410 (2011)
collapsed the maturity levels to two; the two-implementation bar survived as
the criterion for full Internet Standard. RFC 7127 (2014) admits most popular
protocols remain at Proposed Standard, treated as production-grade because of
the review they get. What replaced the formal gate day to day: culture and
events, "rough consensus and running code," IETF Hackathons.
<https://www.rfc-editor.org/rfc/rfc2026>
<https://www.rfc-editor.org/rfc/rfc6410>

W3C keeps the same bar alive: Candidate Recommendation is explicitly the call
for implementations; the operative bar for Recommendation is two independent
interoperable implementations of each feature, evidenced by implementation
reports. <https://www.w3.org/policies/process/#candidate-rec>

### POSIX

No formal implementation gate at ratification because causality ran the other
way: implementations preceded every line. Validation was outsourced to
procurement: FIPS 151 made NIST-validated conformance commercially
load-bearing. The implementation requirement was implicit in the method and
enforced afterward by test suites plus purchasing mandates.

### OCI

runc effectively was the validation, donated on day one; the charter then
deliberately demoted it (spec plus suites judge compliance, not runc). No
written implementation-count requirement for 1.0, because every maintainer
voting was also shipping an implementation. Distribution-spec is the purest
case: 40+ billion pulls of the de facto protocol before the standardization
project opened.

### LSP: no gate, and the bill for it

VS Code was the de facto reference client; adoption stood in for validation.
The documented costs, from language-server authors themselves:

- Michael Peyton Jones (Haskell): code-lens placement unspecified, so Emacs
  renders at line end and VS Code inline, and servers that assumed VS Code
  broke elsewhere; completion-item field semantics "require
  reverse-engineering from VSCode behavior rather than specification text";
  capability registration "wildly inconsistent."
  <https://www.michaelpj.com/blog/2024/09/03/lsp-good-bad-ugly.html>
- matklad (rust-analyzer): UTF-16 column offsets that every non-VS-Code
  implementation pays for; requests shaped around editor widgets rather than
  language concepts. <https://matklad.github.io/2023/10/12/lsp-could-have-been-better.html>

The pattern: with no conformance suite and no second reference client of equal
weight, the dominant implementation's behavior silently becomes the spec,
Hyrum's Law operating exactly as stated, and exactly the failure the
two-implementation rules exist to prevent. LSP succeeded anyway because one
vendor controlled both the spec and the dominant client. That is a validation
strategy, just not a neutral one.

### The failure cases

- **OSI vs TCP/IP** (Andrew Russell, IEEE Spectrum 2013): designed by ISO
  committee from 1977, architecture first, implementations later; the 1988 US
  GOSIP mandate required OSI for federal purchase, and it still lost, because
  OSI products were incomplete and expensive while TCP/IP ran, free, in
  Berkeley Unix. The 1992 IETF Kobe revolt over adopting OSI's CLNP is what
  crystallized "rough consensus and running code."
  <https://spectrum.ieee.org/osi-the-internet-that-wasnt>
- **CORBA** (Michi Henning, ACM Queue 2006): the OMG required no reference
  implementation, and consequently "published standards that turned out to be
  partly or wholly unimplementable because of serious technical flaws."
  <https://queue.acm.org/detail.cfm?id=1142044>

### The refined principle

A first implementation validates implementability. Only a second, independent
one validates that the document, rather than shared tribal knowledge or a
shared codebase, carries the interface. That is what RFC 2026's per-feature
wording and W3C's CR-exit criterion operationalize, and what LSP's per-feature
ambiguities demonstrate by absence. "Done" for a spec is empirically: a
stranger built a compatible implementation from the text alone, feature by
feature. Every durably successful spec in this research had implementations
before or simultaneous with the text.

---

## 5. The publication moments

What concretely existed when each project went public:

- **Linux**: the 25 Aug 1991 comp.os.minix post preceded any code and was a
  requirements survey posted to the incumbent's own user community ("I'd like
  to know what features most people would want"). Linux 0.01 (17 Sep 1991):
  10,239 lines, booted, multitasked, ran bash 1.08 and gcc 1.40. Release
  cadence was ferocious (0.01 Sep, 0.02 Oct 5, 0.11 Dec 8, 0.12 Jan/Feb 92).
  First outside contributor (Ted Ts'o) within weeks; patches returned by email
  appeared in the next tarball in days; credit was social (release notes)
  before the CREDITS file existed (1994). The GPL switch (0.12, Feb 1992)
  guaranteed contributions stayed open; Torvalds later called it "definitely
  the best thing I ever did." Published: implementation only; the spec Linux
  implemented was POSIX, someone else's.
  <https://lwn.net/Articles/928581/>
  <https://en.wikipedia.org/wiki/History_of_Linux>
- **POSIX**: started as a user revolt against lock-in (/usr/group standards
  committee 1981, published 1984), merged into IEEE P1003 in 1985 partly
  because a trade association's document could not become a national standard,
  which government procurement needed. Teeth came from FIPS 151: vendors
  staffed the working groups because certification was worth government
  contracts. Published: spec only, but codifying roughly thirty already-running
  implementations that predated it. <https://www.jimisaak.com/posix-impact>
- **Kubernetes**: first commit 6 June 2014, announced 10 June at DockerCon,
  47,501 lines, running but explicitly pre-production, announced early to win
  a four-way orchestrator land-grab. Partners were recruited before
  announcing: Red Hat met Google in March 2014, three months pre-launch, and
  joined the public launch; by July 2014 Microsoft, IBM, Docker, CoreOS were
  publicly in. 1.0 plus CNCF donation July 2015: announce with running
  prototype and pre-recruited partners, build in the open 13 months, donate to
  a neutral foundation at 1.0. Published: implementation; the specs (CRI etc.)
  crystallized years later. <https://www.redhat.com/en/blog/red-hat-chose-kubernetes-openshift>
- **OCI**: triggered by the appc/rkt challenge to Docker's unilateral control;
  the Linux Foundation brokered the peace. Day one (22 June 2015): draft spec
  plus runc donated simultaneously, appc maintainers seated, ~20 founding
  companies. Specs hit 1.0 two years after the code. Published: both, same day.
  <https://www.docker.com/blog/open-container-project-foundation/>
- **LSP**: published 27 June 2016 after about seven months of production use
  inside shipping VS Code, jointly with Red Hat (committing a Java language
  server) and Codenvy (Eclipse Che integration), each partner being a second
  and third independent client on day one. Working language servers existed at
  publication. The M x N to M + N economics did the recruiting afterward.
  Published: spec plus implementations simultaneously.
  <https://www.redhat.com/en/about/press-releases/red-hat-codenvy-and-microsoft-collaborate-language-server-protocol>
- **Matrix**: announced Sept 2014 at TechCrunch Disrupt with Synapse
  open-sourced and an initial spec. Around ten salaried Amdocs engineers
  2014-2017; funding cut 2017; spec 1.0 only June 2019, alongside the
  Matrix.org Foundation; sustained by successive investments and government
  deployment contracts. The cautionary datapoint: implementation plus spec
  plus a funded full-time team, and still five years to 1.0, perpetually
  funding-constrained. An open protocol with no procurement stick and no
  vendor consortium grows slowly. Published: both, implementation deliberately
  first. <https://matrix.org/blog/2019/06/11/introducing-matrix-1-0-and-the-matrix-org-foundation/>

---

## 6. What keeps people contributing afterward

- **Kubernetes/CNCF**: all work chartered into SIGs, each with elected leads,
  a place to show up with named humans in charge; an explicit contributor
  ladder (member, reviewer, approver); vendor-neutral governance as the actual
  recruiting pitch (Red Hat's stated reason for committing engineers was
  Google's commitment to open meritocratic governance; the CNCF donation
  converted "Google's project" into infrastructure competitors could safely
  invest in). CNCF supplies the non-engineering half: neutral IP home, events,
  certification, which keep the companies who pay the contributors attached.
  <https://github.com/kubernetes/community/blob/master/governance.md>
- **IETF/W3C**: participation formally individual, practically
  employer-funded; vendors write the protocols their products need. RFC 2418
  gives a concrete floor for a viable working group: four or five people in
  management roles plus one to two dozen active contributors.
  <https://www.rfc-editor.org/rfc/rfc2418.html>
- **OCI**: Linux Foundation project, maintainers overwhelmingly from member
  companies, elected Technical Oversight Board. The charter's deliberately
  minimal scope is itself a retention mechanism: companies keep participating
  because the standard cannot be weaponized to swallow their products.
- **Linux year one**: it ran and was fun; the patch-to-merged loop was days;
  Linus credited contributors publicly; comp.os.minix was a pre-assembled
  audience with a live grievance (Tanenbaum rejecting patches); the GPL
  guaranteed contributions could not be enclosed. 100+ developers by 1993.

The conversion mechanism, identical everywhere: **a runnable thing** (try it
in under an hour), **a fast feedback loop** (patch visible in days), **credible
neutrality and credit** (GPL, foundations, named credit), and **an existing
venue full of the right people** (comp.os.minix, DockerCon, DevNation).

Has a spec alone, no running implementation, ever attracted collaborators? No
clean positive example found. OSI is the canonical negative despite massive
government backing. POSIX is the closest to a spec-only success and is the
exception that defines the rule: the implementations already existed and
disagreed, the spec converged them, and a procurement stick forced adoption.
Both standards bodies' own processes encode the assumption that a spec is not
credible until independent implementations interoperate.

---

## 7. Unverified or partially verified items

Flagged by the research passes, kept honest here:

- POSIX sigaction and pthreads invention stories: probable, no primary source
  fetched.
- POSIX Rationale recording formal ballot tallies: not confirmed (rejected
  alternatives prose: confirmed).
- Hypernetes as a CRI pressure source: secondary references only.
- Tim Hockin's "why not libnetwork" post: known to exist, not fetched.
- Matrix state-resolution-v2 revision history and the v1.1 release date:
  mechanism verified from the spec, dates not independently confirmed.
- VSX test suite details: lightly sourced.
- W3C dropped-feature-at-CR concrete examples: process text verified, no
  specific case fetched.
- SOAP/WS-* as a design-by-committee failure: plausible, not researched.
- Exact Linux 0.12 date (sources conflict, mid-Jan vs 7 Feb 1992).
- Count of language servers on LSP publication day.
- "Hypergrowth interface literature" as a named body of work: does not exist;
  the real neighbors are Hyrum's Law and Gall's Law, both cited above.

---

## 8. Open questions this raises for Mojo

Questions, not decisions; the decisions land in roadmap.md and the
standard-shaping document after discussion.

1. Is the existence pass's finish line "complete seam list" or "good enough to
   build against"? The precedent record says the survey is legitimate
   discovery (the /usr/group 1984 analog) but cannot validate the written
   contract; only building has ever done that.
2. Does the phase ordering invert: spec-drafting interleaved with building a
   crude first system, rather than the document gating the build?
3. What is MSI-1's real done-gate? The record's answer: a stranger built a
   second, independent implementation from the text alone, per feature. That
   gate cannot be reached solo by definition.
4. Do the three candidate gaps in the six problems (partial failure,
   performance physics, independent administration) amend seam-method.md, or
   get named as explicitly out of scope for MSI-1?
5. Which of the adoptable mechanics get committed to up front: the POSIX
   undefined/unspecified/implementation-defined triad, a Rationale companion,
   Matrix's no-change-without-working-implementation rule, structure by
   implementer role?
