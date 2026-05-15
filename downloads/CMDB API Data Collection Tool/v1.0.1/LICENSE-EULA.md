# End-User Licence Agreement (EULA) — v1.0.1

> **STATUS: FINAL.** This document is the current End-User Licence Agreement
> for the CMDB API Data Collection Tool v1.0.1 (released May 15, 2026).
> It captures the commercial licensing model and legal terms.
> For changes to licensing terms, contact [License@audittoolkitlabs.com](mailto:License@audittoolkitlabs.com).
> All parties are bound by this Agreement upon Software use.

---

## 0. Parties and Definitions

This End-User Licence Agreement ("**Agreement**") is entered into
between **Michael Churchill trading as AuditToolkitLabs** of 4th Floor, Silverstream House, 45 Fitzroy Street, Fitzrovia, London W1T 6EB ("**Licensor**")
and the natural or legal person who installs, accesses, copies, or
otherwise uses the Software ("**Licensee**"). This Agreement is effective
upon Software installation and use.

Effective Date (for v1.0.1): May 15, 2026

In this Agreement the following definitions apply:

- **"Software"** — the CMDB API Data Collection Tool computer program
  in object or source form, together with any accompanying
  documentation, sample data, and updates supplied by the Licensor.
- **"Installation"** — a single deployed instance of the Software
  running on a server, virtual machine, container, or cluster that the
  Licensee controls, identified by a unique Licence Key.
- **"Workspace"** — a logical data partition inside a single
  Installation. Workspaces are implemented internally by the
  ``Tenant`` data model. A Workspace may correspond to a department,
  business unit, environment (e.g. production, staging), or — under
  the MSP and OEM tiers only — a Managed Customer.
- **"Managed Customer"** — a third party for whose benefit the
  Licensee operates the Software in exchange for any form of
  consideration, whether monetary or non-monetary.
- **"Licence Key"** — the alphanumeric token, file, or environment
  variable (`LICENSE_TIER`) issued by the Licensor that identifies
  the Tier purchased.
- **"Tier"** — one of *Standard*, *Workgroup*, *MSP*, or *OEM* as
  defined in §3 below.
- **"Endpoint"** — any host, virtual machine, container, network
  device, or other system from which the Software collects data.

## 1. Grant of Licence

Subject to the Licensee's continuous compliance with this Agreement
and payment of all applicable fees, the Licensor grants to the
Licensee a **non-exclusive, non-transferable, non-sublicensable**
licence to install and use the Software on **a single Installation**
under the Tier identified by the Licence Key, for the **internal
business purposes** of the Licensee, for the term set out in §6.

### 1.2 Permitted Purpose

The Software is licensed solely for the purposes of:

(a) collecting, storing, analysing, and reporting configuration, inventory,
    and security-related metadata for the Licensee's own IT environments; and
(b) activities reasonably ancillary to those purposes.

This Agreement applies to all installations of CMDB API Data Collection Tool
v1.0.1 and later releases (unless superseded by a subsequent agreement).

Any use of the Software outside this purpose — including surveillance of
individuals, employee monitoring beyond legitimate IT operations, or use
in regulated environments without appropriate authorisation — constitutes
a material breach of this Agreement.

## 2. Restrictions

The Licensee shall not, and shall not permit any third party to:

(a) use the Software outside the Tier purchased;
(b) operate more Workspaces or collect from more Endpoints than the
    Tier permits;
(c) make the Software, or any output uniquely produced by the
    Software, available to any Managed Customer except as expressly
    permitted by the *MSP* or *OEM* Tier;
(d) rent, lease, lend, sell, sublicense, assign, distribute, publish,
    transfer, or otherwise make the Software available to any third
    party, whether on a stand-alone basis or as part of any product
    or service offering;
(e) reverse engineer, decompile, disassemble, or otherwise attempt to
    derive the source code of any object-code components, except to
    the extent such activity is expressly permitted by applicable law
    notwithstanding this limitation;
(f) remove, alter, or obscure any proprietary notice, label, or mark
    on the Software;
(g) use the Software to develop, train, fine-tune, or evaluate any
    competing product;
(h) circumvent, disable, interfere with, or attempt to bypass any
    licence-enforcement, telemetry, usage-limiting, activation,
    integrity-checking, or security-related mechanism of the Software,
    whether by:
    (i) code modification, patching, hot-patching, or injection;
    (ii) manipulation of build artefacts, configuration files, or
         environment variables;
    (iii) use of wrappers, shims, loaders, or modified runtimes; or
    (iv) operation in a deliberately misrepresented or falsified
         execution environment.

## 3. Commercial Tiers

The Tier is selected at purchase and configured in the Installation
via the `LICENSE_TIER` environment variable (or future Licence Key
file). The Tiers are:

### 3.1 Standard (default on-premise)

- One (1) Workspace per Installation.
- Used by a single legal entity for that entity's own internal IT
  asset management, on hardware or cloud infrastructure controlled by
  that entity.
- Workspace separation by department, business unit, or environment
  is **not** available at this Tier; the Licensee may purchase the
  *Workgroup* Tier for that purpose.
- One Installation per purchased licence; additional environments
  (e.g. dev, DR, test) require additional Installations and
  additional licences, subject to any non-production discount the
  Licensor may offer.

### 3.2 Workgroup

- Up to ten (10) Workspaces per Installation.
- Intended for a single legal entity that wishes to partition its own
  data across departments, business units, or environments inside a
  single Installation.
- Workspaces under the Workgroup Tier shall **not** correspond to
  Managed Customers; doing so requires the *MSP* Tier.

### 3.3 MSP (Managed Service Provider)

- Unlimited Workspaces per Installation.
- Intended for a Licensee that operates the Software for the benefit
  of one or more Managed Customers.
- Each Workspace shall correspond to no more than one Managed
  Customer at any time.
- Per-Endpoint royalty fees apply, calculated and payable as set out
  in §4.
- The Licensee shall maintain accurate records of (i) the number of
  Managed Customers, (ii) the assignment of Workspaces to Managed
  Customers, and (iii) the number of Endpoints under management,
  and shall make those records available to the Licensor on request,
  subject to the audit right in §7.
- The Licensee shall not represent the Software, or any user
  interface element of the Software, as the Licensee's own product;
  it shall remain identifiable as Licensor's product, retaining the
  Licensor's marks and notices, unless the *OEM* Tier has been
  purchased.

### 3.4 OEM / Embedded

- Unlimited Workspaces per Installation.
- Permits redistribution of the Software, embedding the Software in
  the Licensee's own product, white-labelling the Software, and any
  other commercial use not permitted under the Standard, Workgroup,
  or MSP Tiers.
- Available **only** under a separately negotiated written agreement
  signed by both parties; configuring `LICENSE_TIER=oem` without
  such an agreement is a material breach of this Agreement.

## 4. Fees, Payment, and MSP Royalty

(a) Licence fees and any MSP royalty rates are set out in the
    Licensor's then-current price list or in an order form executed
    between the parties.
(b) Under the *MSP* Tier the Licensee shall, within 30 days of
    each calendar quarter end, submit to the Licensor a usage
    report stating the number of Endpoints under management on the
    last day of that quarter, and shall pay the corresponding
    royalty within 30 days of invoice.
(c) All fees are exclusive of value added tax, sales tax, or
    equivalent, which the Licensee shall pay in addition where
    applicable.
(d) Late payments accrue interest at the rate provided under the
    Late Payment of Commercial Debts (Interest) Act 1998, as
    amended, or the maximum permitted by law, whichever is the lower.

## 5. Telemetry and Licence Validation

(a) The Software may, if configured by the Licensee, transmit to the
    Licensor periodic non-sensitive telemetry comprising (i) the
    Tier, (ii) the number of Workspaces, (iii) the number of
    Endpoints, and (iv) the Software version. **No collected
    inventory data, host data, vulnerability data, secret, or
    personally identifiable data is transmitted.**
(b) Telemetry is **disabled by default**. The Licensee may enable it
    via `ENABLE_TELEMETRY=true`. Disabling telemetry does not
    affect the Licensee's obligation to comply with this Agreement,
    including the audit right in §7.
(c) Air-gapped Installations may rely on offline Licence Key files;
    the Licensor shall provide such files on request.
(d) The Licensee shall not deliberately suppress, falsify, or
    manipulate telemetry, usage records, or audit artefacts provided
    to the Licensor.
(e) Provision of materially false or misleading usage information
    constitutes a material breach and may result in immediate
    termination under §6(b).

## 6. Term and Termination

(a) This Agreement commences on the date the Licensee first installs
    or uses the Software and continues for the period for which fees
    have been paid (the **"Subscription Term"**).
(b) Either party may terminate immediately on written notice if the
    other party commits a material breach that is not remediable, or
    that is remediable but not remedied within 30 days of notice.
(c) On termination the Licensee shall (i) cease all use of the
    Software, (ii) uninstall and destroy all copies of the Software
    and Licence Keys in its possession or control, and (iii) certify
    such destruction to the Licensor in writing on request.
(e) Where the Licensor reasonably believes that the Software is being
    used in a manner that poses a significant security, legal, or
    reputational risk, the Licensor may require the Licensee to
    immediately suspend use of the affected Installation pending
    investigation.

(f) Sections that by their nature survive termination — including
    §2 (Restrictions), §4 (Fees, in respect of fees accrued before
    termination), §7 (Audit), §8 (Warranty), §9 (Liability), §10
    (Confidentiality), and §13 (Governing Law) — shall so survive.

## 7. Audit Right

The Licensor may, on at least 14 days' written notice and not
more than once in any twelve-month period (or more frequently where
the Licensor reasonably suspects breach), audit the Licensee's use of
the Software, either remotely or at the Licensee's premises during
normal business hours. The Licensee shall co-operate in good faith
with such audit, including by providing access to relevant records,
configuration, telemetry exports, and Licence Key files. If an audit
reveals under-payment of more than 5%, the Licensee shall
reimburse the reasonable costs of the audit in addition to paying
the under-paid fees, with interest in accordance with §4(d).

## 8. Limited Warranty

The Licensor warrants that, for 90 days from delivery, the
Software will perform substantially in accordance with the
documentation supplied with it. The Licensee's exclusive remedy and
the Licensor's entire liability under this warranty is, at the
Licensor's option, (i) to use commercially reasonable efforts to
correct any non-conformity, or (ii) to refund the licence fees paid
in respect of the period of non-conformity. **Except as set out in
this §8, the Software is provided "as is" without warranty of any
kind, whether express or implied, including without limitation any
warranties of merchantability, fitness for a particular purpose,
title, accuracy of data, or non-infringement.**

## 8A. No Reliance on Outputs

The Software generates outputs that are informational and
context-dependent, including reports, indicators, assessments, and
other analytical results derived from data provided to or collected by
the Software.

The Licensee acknowledges and agrees that:
(a) such outputs are provided for decision-support purposes only;
(b) the Software does not provide professional, operational, security,
    legal, or compliance advice;
(c) the Licensee remains solely responsible for independently
    reviewing, validating, testing, and assessing any conclusions,
    recommendations, or actions undertaken on the basis of the
    Software's outputs; and
(d) actions taken by the Licensee in reliance on the Software's outputs
    are undertaken at the Licensee's own risk.

The Licensor makes no representation or warranty that the Software's
outputs will be accurate, complete, up-to-date, or suitable for any
particular decision, remediation, or operational action.

## 9. Limitation of Liability

To the maximum extent permitted by applicable law:

(a) Neither party shall be liable for any indirect, incidental,
    special, or consequential loss or damage, or for any exemplary
    damages, or for loss of profits, revenue, business, goodwill, or
    data, however arising and whether under contract, tort (including
    negligence), or otherwise.
(b) Each party's total cumulative liability arising out of or in
    connection with this Agreement shall not exceed the licence fees
    paid by the Licensee to the Licensor in the twelve months
    immediately preceding the event giving rise to the liability.
(c) The Licensee acknowledges that breach of §§2, 3, or 5 may cause
    irreparable harm for which damages alone may be an inadequate
    remedy, and that the Licensor shall be entitled to seek injunctive
    or equitable relief in addition to any other remedies available.
(d) Nothing in this Agreement excludes or limits liability that
    cannot be excluded or limited as a matter of applicable law,
    including under the Unfair Contract Terms Act 1977, the Consumer
    Rights Act 2015, or any other applicable UK statute, including
    liability for death or personal injury caused by negligence and
    liability for fraud or fraudulent misrepresentation.

## 9A. Licensee Indemnity

The Licensee shall indemnify and hold harmless the Licensor (and its
officers, employees, and agents) from and against all claims, losses,
liabilities, damages, fines, penalties, and reasonable costs
(including legal fees) arising out of or relating to:

(a) the Licensee's installation, configuration, deployment, operation,
    or use of the Software;
(b) any actions, remediation steps, or operational changes undertaken
    by the Licensee based on the Software's outputs;
(c) the Licensee's collection, processing, storage, or disclosure of
    data using the Software, including any allegation that such data
    collection or processing was unlawful or unauthorised;
(d) the Licensee's breach of this Agreement or applicable law; or
(e) use of the Software for or on behalf of third parties except as
    expressly permitted under the purchased Tier.

(f) any unauthorised, unlawful, or non-compliant use of the Software,
  including security testing, scanning, access, or data collection
  performed without the required written authority.

This indemnity shall not apply to the extent that a claim arises
solely from the Licensor's wilful misconduct or gross negligence.

## 10. Confidentiality

Each party shall keep confidential the other party's confidential
information disclosed under this Agreement, shall use it only for
the purposes of this Agreement, and shall protect it with the same
degree of care it uses for its own confidential information of like
importance, and in any event with no less than reasonable care. This
clause does not apply to information that is or becomes public
through no breach of this clause, was lawfully known to the
recipient before disclosure, is independently developed without use
of the disclosing party's confidential information, or is required
to be disclosed by law or regulator (subject to prompt notice where
permitted).

## 11. Data Protection and Security

(a) The Software is intended to be operated entirely within the
    Licensee's environment and does not require the Licensor to
    process the Licensee's personal data in the ordinary course.
(b) Where the Licensor does process personal data on the Licensee's
    behalf (for example, while providing support), the parties shall
    enter into a separate Data Processing Addendum on the Licensor's
    standard terms.
(c) The Licensee is solely responsible for the lawful collection,
    storage, and processing of any personal data collected by the
    Software from the Licensee's Endpoints.
(d) The Licensee acknowledges that the Software collects
    information about hosts, software versions, and vulnerabilities.
    Such information is deemed sensitive operational and security data.
    The Licensee acknowledges that unauthorised disclosure could
    materially increase security risk to the Licensee's environment;
    the Licensee shall secure the Installation accordingly, including by
    following the deployment guidance in `docs/04-deployment-guide.md`.

## 11A. Licensee Security Obligations

The Licensee shall:

(a) operate the Software in accordance with generally accepted
    information security practices and the deployment guidance
    provided by the Licensor;
(b) protect the Software against unauthorised access, including
    securing credentials, API keys, Licence Keys, and administrative
    interfaces;
(c) apply security updates and patches provided by the Licensor
    within a reasonable timeframe appropriate to the risk;
(d) ensure that access to collected data is restricted to authorised
    personnel with a legitimate business need;
(e) not intentionally expose the Software to the public Internet
    without appropriate authentication, network protections, and
    monitoring.

Failure to comply with this section constitutes a material breach.

## 11B. Licensee Data Ownership

Nothing in this Agreement transfers ownership of, or grants any
rights in, the Licensee's data to the Licensor, save for the limited
rights necessary to validate licence compliance and provide support
services where agreed.

## 12. Open-Source Components

The Software incorporates third-party open-source components
licensed under their own terms. A list of such components and their
licences is provided in `NOTICE.md`. Nothing in this
Agreement restricts the Licensee's rights under those open-source
licences in respect of those components.

## 13. Governing Law and Jurisdiction

This Agreement is governed by and construed in accordance with the
laws of England and Wales. The parties irrevocably submit to the
exclusive jurisdiction of the courts of England and Wales in respect
of any dispute arising out of or in connection with this Agreement.

## 14. Entire Agreement

This Agreement, together with any order form, schedule, or addendum
expressly incorporated by reference, constitutes the entire
agreement between the parties in respect of the Software and
supersedes all prior negotiations, representations, and agreements,
whether oral or written.

## 15. Assignment

The Licensee shall not assign or transfer this Agreement, in whole
or in part, without the prior written consent of the Licensor. The
Licensor may assign this Agreement freely.

## 16. Changes to this Agreement

The Licensor may update this Agreement from time to time. Material
changes shall be notified to the Licensee in writing or by in-product
notice at least 30 days before they take effect. Continued use
of the Software after the effective date constitutes acceptance of
the updated Agreement.

---

### Acceptance

By installing, copying, or using the Software, the Licensee
acknowledges that it has read this Agreement, understands it, and
agrees to be bound by it. If the Licensee does not agree, it must
not install, copy, or use the Software, and must immediately destroy
any copies in its possession or control.

Michael Churchill trading as AuditToolkitLabs

---

## Appendix A — Tier Summary (non-binding quick reference)

| Tier        | `LICENSE_TIER` | Workspaces | Managed customers | Royalty                    |
|-------------|----------------|-----------:|-------------------|----------------------------|
| Standard    | `standard`     | 1          | Not permitted     | None                       |
| Workgroup   | `workgroup`    | 10         | Not permitted     | None                       |
| MSP         | `msp`          | Unlimited  | Permitted         | Per-Endpoint, see §4       |
| OEM         | `oem`          | Unlimited  | Permitted         | Per separate OEM agreement |

This Appendix is a summary only; in case of conflict the body of
this Agreement prevails.
