# PRD: Candidate Identity Trust in Workable

**Author**: Evangelos Papangelis
**Date**: 2026-11-10
**Status**: Draft for review
**Working name**: Candidate Trust

***

## Summary

An application no longer guarantees a real individual applying once, in their own name. With remote hiring nobody meets the applicant face to face, and generative AI turns out a polished application for next to nothing [S1, S10].

**Recommendation**: add identity trust to the Workable pipeline one layer at a time, with people making every decision. Phase 1 puts automated identity risk signals, with their reasons, on each application, and gives recruiters a place to log what they notice themselves. Phase 2 lets employers request identity verification from a partner service at a stage they choose. Phase 3 keeps the verified identity with the candidate up to offer and onboarding. Workable raises the concern and explains it. Recruiters decide. Nobody is rejected automatically.

**First target**: recruiters filling remote roles that draw many applicants, mostly from job boards. We begin at application and screening, where volume peaks, signals are cheapest to collect, and each interview avoided gives the most time back.

## Key terms: manual and automated signals

A **signal** is any observation that raises or lowers doubt about whether an applicant is who they say they are. No signal proves anything alone, and the product never acts on one in isolation [S1, S9]. Signals come from two places. **Manual signals** are what a recruiter or interviewer notices and judges during a screening call or interview. **Automated signals** are checks that Workable's system runs on application data and its metadata.

### Manual signals recruiters use today

| Group | What the recruiter notices | Source |
| :- | :- | :- |
| Face and voice on video | Facial movement that looks unnatural, speech slightly out of step with the lips, blur or flicker around the jawline and hairline, an expression that seems off, such as a smile that never reaches the eyes | [S1, S8, S11] |
| Lighting | Shadows that do not follow the head, skin tone that shifts as the person turns, a face lit differently from the room around it | [S11] |
| Camera reluctance | Keeps the camera off, or declines to change the angle, turn sideways, or show the full face. Profile views are harder for a deepfake to hold | [S1, S8, S11] |
| Depth of experience | Cannot talk through a real project: why they decided as they did, which alternatives they weighed, what limits they faced, what went wrong. Offers polished generalities in place of named tools, team dynamics, and results | [S2, S8, S11] |
| Unplanned questions | Hesitates over easy local questions, such as today's weather or the nearest lunch spot to their stated office, which someone actually there answers at once | [S1, S11] |
| Delivery | Answers that sound rehearsed, with no pauses, filler words, or self-corrections. Vagueness when pressed for detail, for example naming a data migration but not its source system, target, or team size. Eyes that seem to follow a script off screen, especially on technical questions | [S8, S11] |

### Automated signals

| Group | What the system checks | Source |
| :- | :- | :- |
| Contact details | An email address or portfolio site that is newly created or on an unusual domain. An internet-based phone number where a local one is expected, or a number that does not fit the stated location | [S1, S3, S4, S12] |
| Network and device | A VPN, proxy, or rented server. One IP address behind applications from supposedly different people. Device or network changes between stages | [S1, S5, S9, S12] |
| Application pattern | One person under several profiles. Many reworked copies of a single resume. Several unrelated applications within 48 hours. A resume that mirrors the job post too closely, or a file created minutes ago | [S1, S8] |
| Consistency of claims | Locations that disagree between the application, the resume, and the interview. Dates, roles, or locations that conflict with LinkedIn or public records. Overlapping full-time jobs. A photo that appears elsewhere online in an unrelated context | [S3, S4, S8, S12] |
| Thin footprint | Little online presence for the seniority claimed | [S1, S8] |

Recruiters should not need to search for any of this themselves. Even a manual routine of checking what kind of phone number was given, comparing public profiles, and noticing IP addresses that recur across submissions already uncovers a good part of the fraud [S12]. Phase 1 therefore starts with these checks.

Points to keep in mind:

- Manual signals are judgment and can mislead. A camera kept off may reflect poor bandwidth, privacy, or a disability. Pauses and filler words vary with language and temperament. A glance aside may be someone checking notes. Questions and camera requests should not put at a disadvantage candidates who face language, sensory, or technology limits [S10].
- Asking every candidate for a role the same core questions makes answers comparable, so a very different one stands out [S5, S11].
- A logged observation carries the recruiter's name and the time, can be cleared like any other flag, and is never the only reason to reject [S9].

## 1. Problem and scope

### Problem statement

Recruiters lose screening and interview hours to applicants who are someone other than they claim: the identity is invented, taken from another individual, submitted several times, or fronted by a stand-in. At worst, such a hire is handed company systems and data. The ATS gives recruiters no organised way to spot identity risk, handle it the same way each time, or log the outcome. Checking by hand works but breaks down once an opening draws hundreds of applicants [S4].

### What this PRD treats as identity fraud

Academic work offers a practical test. Is there a genuine individual behind the application, with real skills that hiring can evaluate [S10]?

- **In scope**: the identity is invented, stolen, duplicated, or swapped midway.
- **Out of scope**: genuine applicants who overstate their experience or let AI draft their resume. That concerns fit for the role, which normal screening already covers [S10, S2].

The boundary protects honest candidates. In one US survey, 64.2% of workers said their resume had contained a lie at some point [S7]. In another, 67% of employers were comfortable with applicants getting help from generative AI [S6]. Calling either behaviour fraud would flag the bulk of applicants.

## 2. Types of fraud and which are in scope

| Type | What happens | Stage | Scope decision |
| :- | :- | :- | :- |
| Bulk and repeat applications | Software applies in bulk. One individual uses several profiles or sends many reworked copies of a single resume [S1, S8] | Application | **In, Phase 1** |
| Invented identity | A persona with made-up contact details, a purpose-built LinkedIn profile, a generated photo, and no other online trace [S3, S10] | Application, screening | **In, Phase 1** (signals), **Phase 2** (verification) |
| Stolen identity | Somebody applies with a real individual's details, often from behind a VPN [S5, S6] | Application through offer | **In, Phase 2** |
| Stand-ins at interview | A substitute or a deepfake takes the interview, and somebody else turns up for work [S5, S6, S8] | Interview, onboarding | **Partly in, Phase 3** (same-person checks only) |
| Swap after the offer | Whoever completes onboarding is not the individual interviewed [S9] | Offer through onboarding | **In, Phase 3** |
| Cheating on assessments | The test is taken by somebody else or with banned help [S5, S8] | Assessment | **Out**. Proctoring belongs to assessment partners |
| False credentials and references | Degrees, past employers, or referees that are not real. A paid fake referee averages $128.60 [S7] | Screening through offer | **Out**. Background check providers cover this |
| Exaggeration and AI help | A genuine applicant inflates their skills or has AI produce the resume [S2, S7] | Application | **Out**. The identity is real |

Coordinated and state-linked groups mix these methods. The North Korean IT worker cases pair identities taken from US citizens with VPNs and deepfakes [S6, S10]. The consequences are graver, but the defences are those in this table.

## 3. Users, use cases, and workflow points

| User | What they need | Priority |
| :- | :- | :- |
| **Recruiter** | See early which applications deserve extra scrutiny, and which step to take | Primary |
| **Talent acquisition lead or account admin** | Apply one policy per role, and learn which channels send doubtful applicants | Primary |
| **Hiring manager or interviewer** | Trust that the applicant and the interviewee are one and the same | Secondary |
| **Security, IT, and compliance** | A record of checks and decisions, plus assurance on sensitive roles ahead of any access | Secondary (Phase 3) |
| **Genuine candidate** | Avoid being flagged by mistake, know what a check involves, and be able to challenge the result | Protected in every phase |

| Stage | Main risk | How Workable could help | Phase |
| :- | :- | :- | :- |
| Sourcing and application | Automated submissions, repeat applicants, invented profiles | Risk signals, linking of repeat applicants, trust report by channel | 1 |
| Screening | Thin or conflicting identity details | Review panel with reasons, actions to settle flags | 1 |
| Ahead of the first interview or the offer | Stolen or invented identity | Identity verification by a partner, requested when needed | 2 |
| Interview | A stand-in attends | Verified status for interviewers, short guide | 3 |
| Offer and onboarding | A different individual joins | Identity confirmed again at handover, record exported | 3 |

## 4. Evidence, assumptions, hypotheses, research needs

### Evidence and its strength

| Claim | Source | Strength |
| :- | :- | :- |
| A July 2026 screening run over 70,280 US-based candidates marked 46.2% for a second look. Queried, not proven | S1 | Weak to medium. A single vendor's marketplace and flag definition, published as marketing |
| Aggregators and job boards produced flags about 1.8 times as often as paid social | S1 | Weak to medium. Same limits |
| In certain hiring rounds, over 30% of applicants were judged suspicious | S3 | Weak. A vendor's anecdote |
| One HR vendor's customers put fraud in interviews at 10% to 30%, especially in engineering | S6 | Weak. A vendor executive relaying clients |
| Gartner expects one in four applicants may be fake by 2028 | S6 | A prediction, not measured data |
| More than 300 US firms took on fraudulent workers unknowingly. Some kept their access for 4 to 6 months | S10 (citing a legal advisory) | Medium. Reported second hand |
| In January 2025 the US Justice Department charged five people over North Korean IT worker schemes | S6 | Strong. Public legal record |
| A poor hire is said to cost around 30% of first-year salary. Estimates for a fake employee run from $240,000 to $850,000 | S6, S8 | Weak. Much quoted, seldom sourced |
| Pairing people with a detection model beat using only one of them. When the model erred, human accuracy fell 2.7% | S10 (citing Groh et al., 2022) | Medium. A real experiment, but on deepfake video, not hiring |
| With very large applicant numbers, even a precise detector can swamp reviewers with false alarms | S10 | Reasoning only, no data |

The evidence has two weaknesses. Nearly every prevalence figure comes from a company with a product to sell, and the two academic papers propose frameworks without testing them on data [S9, S10]. No source reports a verified fraud rate, and fraud that succeeds never appears in a dataset [S10].

### Assumptions to confirm inside Workable

- A1. Workable customers are mainly small or mid-sized with no security team of their own, so the feature must work well on default settings.
- A2. Job boards deliver a large part of Workable's applications. In S1 that channel produced the most flags.
- A3. Workable already stores usable data: when each application arrived, contact details, the resume file, its channel, and a candidate's history across jobs in an account.
- A4. The mechanism that connects background check and assessment providers to Workable can also carry an identity verification partner.
- A5. Customers will tolerate a "needs review" label that is occasionally mistaken, provided it gives reasons and is quick to clear.

### Hypotheses to test

- H1. What is visible at application (repeat applicants, contact detail validity, how it was submitted) catches the bulk of the identity fraud recruiters go on to confirm.
- H2. Recruiters respond more consistently to a flag that lists its reasons than to an unexplained score.
- H3. Few genuine candidates drop out when verification is requested only at a chosen stage, since it reaches a small group who have already put effort in.
- H4. Seeing trust levels per channel leads customers to move their job advertising spend.

### Research still needed

- R1. Measure the problem in Workable's own data: repeat and multi-profile applicants, and rejection notes that already mention fake candidates, split by role type and channel.
- R2. Talk to 12 to 15 recruiters and admins across segments (tech, staffing agencies, high-volume hiring) about what they check by hand, which manual signals they rely on, and how much friction they would accept.
- R3. Assemble a set of confirmed cases with a handful of design partners, so signal precision can be measured ahead of launch.
- R4. Get a legal review per market on automated decisions, biometric data, and AI in employment (needs verification by counsel).
- R5. Research with candidates: how genuine applicants feel when asked to verify, and which groups a check might shut out.

## 5. Competitive analysis

Drawn from what vendors publish about themselves. Nothing has been tested hands-on.

| Player | Type | What they do | Why it matters |
| :- | :- | :- | :- |
| **Greenhouse** | ATS | Partner CLEAR checks a government ID and a biometric match inside the hiring flow. AI is presented as a helper that spots repeated applications from one IP address and resumes that contradict online profiles [S5] | Nearest rival. Makes this an expected ATS feature |
| **hackajob (Archer Approves)** | Talent marketplace | Three steps: a single verified profile per job seeker, checks on online signals, and a government ID check that is optional or set by the employer. Reports over 30,000 fake applicants stopped monthly [S1] | Owns the candidate account, unlike an ATS |
| **Hero Parser** | Parsing add-on | Examines phone, email, photo, location, work history, and LinkedIn. Marks items to check and leaves rejection to the recruiter. Says it meets GDPR and AI Act rules [S3, S4] | Single-purpose tool for IT hiring |
| **Manatal** | ATS | Checks candidate details against 20+ public platforms, reports unusual pipeline patterns, and adds proctoring via assessment partners [S8] | Reuses existing features. Silent on identity verification |
| **Phenom** | HR technology suite | Said it would release an AI agent that detects deepfakes during interviews [S6] | Enterprise buyers, interview stage |
| **SHRM (shrm.org)** | HR association, not a vendor | Sells no product. Its recent coverage advises employers to move identity, employment, and education checks ahead of the offer, compare application materials, documents, and online profiles, and set clear rules on AI use for candidates [S13] | Reaches HR and talent teams at large employers, so it shapes what big clients expect to see (needs verification). Treats a pre-offer check as normal practice |
| **Neighbouring tools** | Single-purpose products | Assessments with proctoring (HackerRank, Codility, TestGorilla), and checks on background and education (Certn, HireRight) [S8] | One stage each, often late. Nothing links the stages |

**What this tells us**

1. Vendors are settling on one recipe: combine several signals, add an identity check only when needed, and leave the final say to a person [S1, S4, S5].
2. The identity check is moving earlier: once part of the background check after an offer, now ahead of the interview [S1, S5].
3. Three openings remain for Workable: trust by channel for employers, a stated route for candidates to challenge a flag, and ready-made defaults for teams with no security staff. No vendor reviewed explains what happens to a candidate flagged in error.

The sources do not cover Ashby, Lever, SmartRecruiters, Teamtailor, or specialist identity vendors. These still need research.

## 6. Product approaches considered

| Approach | What it involves | In its favour | Against it |
| :- | :- | :- | :- |
| A. Guidance and manual tools | Warning-sign checklists, interview kits, a "suspected fraud" rejection reason | Low cost, quick, little risk | Cannot keep pace with volume. Depends on recruiters staying alert [S4] |
| B. Background signals reviewed by recruiters | Flags with reasons on applications. No automatic action | Finds bulk fraud at the start, asks nothing of candidates, uses data Workable already holds | Signals are estimates: weak on stolen identities, sometimes wrong |
| C. ID check required from everyone | Every applicant verifies a government ID on applying | The most powerful single check [S1] | Heavy for candidates, a cost per application, a privacy load, a barrier for some genuine people. It confirms the document, not the holder's location [S1] |
| D. Detecting deepfakes in interview media | Analysis of live video and sound | Tackles the threat people fear most | Fast-moving technology makes detection a constant chase [S10]. Biometric and legal exposure. Costly to build |
| E. Layers: signals, then verification when needed, then same-person checks | Begins with B. Verification only when policy or risk requires it. The verified identity follows the candidate between stages [S9] | Friction matches risk. Each layer covers the last one's gap. Useful from the first release | Full coverage takes longer. Needs a partner. More to explain |

## 7. Recommendation and trade-offs

**Choose approach E and build it in order: signals first, verification on request second, same-person checks third.**

Reasons:

- **The evidence points this way.** Source after source says one signal proves nothing, and that combined signals reviewed by a person work best [S1, S4, S9, S10].
- **The burden matches the risk.** The great majority of applicants are genuine. Only the few asked to verify feel any friction, and employers can tie the rule to how sensitive a role is [S9].
- **Customers benefit from Phase 1**, which runs on data Workable owns and needs no partner agreement.
- **It plays to Workable's position.** The whole pipeline lives in Workable, so signals from different stages can be joined up, which single-purpose tools cannot do [S9].

Principles for every phase:

1. **A flag is a prompt, not a judgment.** The wording is "Needs review", never "Fraud". The system rejects nobody [S4, S9].
2. **Reasons are shown.** Each flag says in everyday language what triggered it [S9].
3. **Same rule for everyone.** An account policy by role and stage decides who verifies, not a recruiter's hunch about one individual [S9].
4. **As little data as possible.** Workable keeps no biometric data, and signals never use protected characteristics [S9].
5. **Open with candidates.** We tell candidates that checks take place, and they can request a second look [S9].

What we give up:

- Phases 1 to 3 do not spot a deepfake during a live interview. We lower that risk another way, by confirming who the candidate is in advance.
- Skilled fraudsters will get past the signals, and some genuine applicants will be flagged. We accept catching fewer cases as the price of never rejecting automatically.
- Reviewing flags is extra work for recruiters. It has to take less time than the interviews it saves.
- Verification relies on an outside provider, which sets the quality, the countries covered, and the price.

## 8. Scope and phased delivery

Timings are rough, assume a single cross-functional squad, and will be revised once discovery is complete.

| Phase | Aim | What is included | Done when |
| :- | :- | :- | :- |
| **0. Discovery** (4 to 6 weeks) | Size the problem, remove the main unknowns | R1 to R5. Trial candidate signals on past data, unseen by users. Sign up 5 to 8 design partners | Baseline rates, legal position per market, and signal list are known |
| **1. Application signals** (about one quarter) | Let recruiters catch doubtful applications early | Linking of repeat and multi-profile applicants, contact detail checks, submission pattern signals, recruiter observation log, "Needs review" panel with reasons, actions to settle a flag, trust report by channel, notice to candidates. Hidden first, then beta | Flag precision hits target on design partner data. Application completion does not fall |
| **2. Verification on request** (about one quarter) | Confirm who a candidate is when it counts | Partner integration, rules by role and stage, candidate verification flow with a second route, verified status on the profile | Verification completion and drop-off stay inside guardrails |
| **3. Same-person checks** (later) | Ensure whoever is hired is the individual verified | Verified status shown to interviewers, repeat check at offer and onboarding handover, interviewer guide, exportable record for security teams | Defined with design partners in regulated or sensitive industries |

**Explicitly out of scope**: automatic rejection, deepfake analysis of video or audio, assessment proctoring, credential and reference checks, spotting AI-written resumes, monitoring after hire, and passing flagged candidate data between customers (to be reconsidered only following a privacy review).

## 9. Epics and user stories

**Epic 1: Risk signals (Phase 1)**
- As a recruiter, I want profiles belonging to one individual grouped, so that I assess each applicant once.
- As a recruiter, I want a warning when an email address or phone number looks throwaway or dead, so that I can check first.
- As a recruiter, I want to be told when software appears to have sent an application, so that real applicants get my attention first.

**Epic 2: Review experience (Phase 1)**
- As a recruiter, I want each flag to explain itself in simple words, so that I can form my own view.
- As a recruiter, I want to close a flag as cleared, confirmed, or sent for verification, so that my decision is on record.
- As a recruiter, I want a pipeline filter for review status, so that I can work through open flags together.
- As a recruiter or interviewer, I want to log what I noticed, such as reluctance to move on camera, from a fixed list, so that my observation sits beside the automated signals for the rest of the team.

**Epic 3: Policy and controls (Phase 1 to 2)**
- As an admin, I want to switch signals on or off and pick who sees them, so that the feature matches our rules.
- As an admin, I want verification made mandatory for selected roles at a stage I pick, so that everyone applying for a role takes the same step.

**Epic 4: Candidate notice and appeal (Phase 1 to 2)**
- As a candidate, I want to know up front that my identity may be checked, so that the request is no surprise.
- As a candidate, I want to ask a person to review a check that goes against me, so that a mistake does not lose me the opportunity.
- As a candidate, I want another route to verify when the usual one fails me, so that I can still take part.

**Epic 5: Verification on request (Phase 2)**
- As a recruiter, I want to request verification with a single click, so that any doubt is settled ahead of the interview.
- As a candidate, I want verification to take a few minutes on a phone, so that my application keeps moving.
- As a hiring manager, I want to see the verified status when I prepare, so that I know I will be meeting the applicant.

**Epic 6: Measurement and audit (all phases)**
- As an admin, I want a report of how often each channel produces flags, so that I can change where we advertise.
- As a compliance owner, I want every flag and decision logged with the decision maker's name, so that we can respond to an audit or complaint.
- As the product team, we want recruiters' decisions to feed signal tuning, so that flags become more precise with use.

**Epic 7: Same-person checks (Phase 3)**
- As an HR admin, I want identity checked again when the offer is accepted, so that the new joiner is the individual we verified.
- As a security lead, I want the verified status in hand before accounts are created, so that access decisions build on the hiring record.

## 10. Success metrics

Provisional targets are informed guesses that Phase 0 will replace.

| Type | Metric | Target |
| :- | :- | :- |
| **Primary outcome** | Share of confirmed fraud cases found ahead of the first interview | Up every quarter. Phase 0 sets the baseline |
| **Primary outcome** | Interview hours per account that went to candidates later confirmed as fraudulent | Cut by 50% in active accounts (provisional) |
| **Quality** | Flag precision: reviewed flags that recruiters confirm | 60% or more at launch, then rising (provisional) |
| **Adoption** | Proportion of flags a recruiter settles inside 5 working days | 70% or more (provisional) |
| **Guardrail: fairness** | Flagged and cleared rates by candidate country, channel, and career stage | No gap we cannot explain. Checked monthly |
| **Guardrail: candidate experience** | Completion rates for applications and verification. Review requests and overturned flags | Application completion holds steady. Verification completion of 85% or more (provisional) |
| **Guardrail: speed** | Time to hire in accounts that use the feature | Does not get worse |
| **Business** | Uptake in the target segment, mentions in notes on won and lost deals, retention of adopting accounts | Measured only. No target for the first year |

One limit to state plainly: only detected fraud can be counted. What slips through leaves no trace, so recall cannot be measured directly [S10]. Design partner audits and planted test cases come closest.

## 11. Risks, dependencies, and open questions

### Risks

| Risk | Impact | Mitigation |
| :- | :- | :- |
| **Wrong flags and unfair results.** A recently created email, an internet-based phone number, a VPN, or a small online presence are also normal for honest people who are early in their career, guard their privacy, or have just relocated | Honest candidates lose out. Legal risk for customers | No automatic rejection. Reasons on every flag. Fairness guardrail. Route to challenge. Several signals needed per flag [S1] |
| **Regulation.** Markets differ in their rules on automated decisions, biometric data, and AI in employment (needs verification by counsel) | Launch blocked, or liability for Workable | Legal review in Phase 0. A person decides. Biometric data stays with the partner. Region-by-region release |
| **Flags ignored or over-trusted.** Recruiters may skip flags or accept them unthinkingly. A wrong model can lower human accuracy [S10] | No benefit, or biased decisions | Prioritise precision. Neutral wording. Guidance inside the product |
| **A moving target.** Fraud methods change more quickly than product releases [S10] | Signals lose their power | Learn from recruiter decisions. Keep several layers. Never show candidates how detection works |
| **Verification that reassures too much.** A genuine document says nothing about location, or about who ends up doing the job [S1] | Fraud carries a "verified" badge | Explain precisely what verified covers. Add the Phase 3 checks |
| **Candidates put off.** Being asked to verify drives good candidates away | Fewer completed applications | Ask only when needed, once candidates are committed, with a quick mobile flow |
| **Reliance on a partner** | Costs, countries not covered, downtime | Assess two vendors. Build the integration so the partner can be swapped |

### Dependencies

- Legal and privacy review in every market.
- An identity verification partner and commercial terms for Phase 2.
- The data and ML platform, for computing signals and taking in feedback.
- The teams behind the application form, candidate profile, reporting, and onboarding handover.
- Customer success and support, ready for launch and for candidates' review requests.
- Design partners who will label the cases they confirm.

### Open questions

1. How big is the problem in Workable today, by segment and channel? (R1)
2. Does Phase 1 belong in the core product or in a paid add-on, and who covers each verification?
3. Should everyone on a hiring team see the signals, or only recruiters and admins?
4. Who deals with a candidate's request for review: the employer, Workable, or the verification partner?
5. Is it lawful to use signals across accounts, for example one invented profile applying to many Workable customers? This may be both the most powerful signal and the hardest privacy issue.
6. Do staffing agencies belong in the launch segment? Their reputation suffers more than most if a fake candidate reaches a client [S8].
7. In markets where the partner supports few document types, what should "verified" mean?

***

## Sources

| Key | Source | File |
| :- | :- | :- |
| S1 | hackajob, "Fake candidates are flooding remote hiring" (Sep 2026) | hackajob_com_employer_blog_fake-candidates-are-flooding-remote-hiring-here-s-how-to-spot-them_1920w_default.png |
| S2 | hackajob, "How to tell if a candidate really has the skills on their CV" (Sep 2026) | hackajob_com_employer_blog_how-to-tell-if-a-candidate-really-has-the-skills-on-their-cv_1920w_default.png |
| S3 | Hero Parser, "Fake candidates detection" product page | heroparser_com_fake-candidate-detection_1920w_default.png |
| S4 | Hero Parser, "How to detect fake candidates before the interview?" | heroparser_com_recruitment_how-to-detect-fake-candidates_1920w_default.png |
| S5 | Greenhouse, "What is candidate fraud, and how can recruiters prevent it?" (Nov 2025) | www_greenhouse_com_uk_blog_what-is-candidate-fraud-and-how-can-recruiters-prevent-it_1920w_default.png |
| S6 | Computerworld, "Job seekers using genAI to fake skills and credentials" (Apr 2025) | www_computerworld_com_article_3964995_job-seekers-using-genai-to-fake-skills-and-credentials_1920w_default.png |
| S7 | StandOut CV, "How many people lie on their resume?" (survey of 2,102 US adults, updated Mar 2025) | standout-cv_com_usa_stats-usa_study-fake-job-references-resume-lies_key-findings_1920w_default.png |
| S8 | Manatal, "How to Identify Fake Candidates in Recruitment" (Jul 2026) | www_manatal_com_blog_how-to-identify-fake-candidates_1920w_default.png |
| S9 | Bableshwar, "Preventing Hiring Fraud and Workforce Risk", Human Resource and Leadership Journal 11(2), 2026 | Preventing_Hiring_Fraud_and_Workforce_Risk_A_Real-.pdf |
| S10 | Patel, "The phantom candidate", Human Resource Management Review 36(4), 2026 | The phantom candidate: A framework for combating AI-enabled identity deception in remote hiring (text file) |
| S11 | Recruiter guidance on spotting deepfake and scripted candidates (Greek-language text supplied by the author, publisher not identified) | Not in raw/. Pasted by the author |
| S12 | Guidance on inconsistent identifiers across applications (English text supplied by the author, publisher not identified) | Not in raw/. Pasted by the author |
| S13 | Roy Maurer, SHRM, "Candidate Deception Poses Growing Risk for Employers" (Oct 2026) | Not in raw/. Fetched from shrm.org |
