# Clinical Trial SDTM Standardization Expert (ct-sdtm)

- **English guide** → [README.md](https://github.com/medstatstar/ct-sdtm/blob/main/README.md) · **中文指南** → [README\_zh-CN.md](https://github.com/medstatstar/ct-sdtm/blob/main/README_zh-CN.md)

<div align="center">
  <img src="assets/icon.svg" width="240" height="240" alt="ct-sdtm logo"/>
</div>

> **Tier: B (confidential input · processed locally · no egress)** | Lifecycle stage: ⑤ Data management (conversion)

Maps sponsor raw data into compliant SDTM datasets per the **CDISC SDTMIG v3.4** standard (mandatory at FDA since 2025-03-15): applies controlled-terminology validation, runs compliance checks, and prepares submission-ready XPT v5 + Define-XML v2.1 source metadata. Also supports reverse CRF field matching, SDRG/ADRG reviewer's-guide drafts, and synthetic test-data generation. **Processed entirely locally; confidential data never leaves the machine.**

---

## How to Use This Skill

This is a **Tier B skill**: the skill package is **not publicly released** (only this document and whitelist files are public; for custom / on-prem deployment, please contact the author). The two usage modes below are therefore **the full-featured edition vs. the shared online edition**:

1. **Conversation prompts (full-featured)**: Load this skill in WorkBuddy or another AI agent and describe your needs in natural language; the skill performs the real mapping locally and reports results. All cases and capabilities described in this document are fully delivered through this mode.
2. **Workbench UI (no installation required)**: Visit the ct-series unified web portal **https://ct.medstatstar.com** directly to use the publicly provided features in your browser. All users share a daily pool of free quota, but data must be sent to the Coze server for processing — please evaluate and control the risk yourself.

---

## Who This Is For

The ct-* family of clinical-trial skills covers needs across the entire clinical-trial lifecycle. ct-sdtm is built for three groups that need to **turn raw collected data into compliant SDTM datasets**:

- **Clinical-trial practitioners at pharmaceutical companies** — sponsors, CROs, and data-management / statistical-programming / medical / regulatory roles: raw → SDTM mapping, controlled-terminology application, and pre-submission compliance checks;
- **Clinicians and nurses who design, manage, or conduct clinical-trial projects**: check how CRF fields correspond to SDTM variables and understand the data-standardization pipeline;
- **Medical students who want to learn clinical-trial methodology in a structured way**: SDTMIG v3.4 domain structure, controlled terminology, and Define-XML submission conventions.

---

## 1. What You Can Do — Capability Scenario Index

Grouped by usage scenario; every row includes a phrase you can **copy verbatim**.

### ① Forward mapping (raw → SDTM)

| Capability | Description | Try saying |
|:---|:---|:---|
| One-shot directory mapping | Auto domain detection, value-level completion, provenance, and compliance checks in one run | "Map the raw data under data/raw to SDTM" |
| Selected domains + study ID | Map only chosen domains with a custom study identifier | "Map only DM and AE, study ID STUDY-ABC-2026" |
| Safe preview | Estimate per-domain rows and issue counts without writing files | "Preview rows and issues per domain, don't write files" |
| Multi-format input | CSV / TSV / Excel / SAS datasets / XPT / JSON | "My raw data mixes xlsx and sas7bdat" |
| Value-level routing | Auto-fill unit-type information by test code; multi-value cases prompt rather than guess | "VS has no unit column — can you fill mmHg by DIABP?" |
| Bilingual labels | Chinese variable-label columns + Define-XML bilingual labels | "Give me a version with Chinese variable labels" |
| Custom column mapping | Column-map file adapts to different EDC export structures | "My EDC column names differ; I have a mapping file" |

### ② Validation & checks

| Capability | Description | Try saying |
|:---|:---|:---|
| Controlled-terminology validation | Enumeration checks, near-miss suggestions, version switching; violations carry rule IDs | "Check whether AESEV and RACE values conform" |
| Dataset compliance check | CORE rule set + model-level general rules + column-order / structure / value-level checks | "Run a full SDTM compliance validation" |
| Cross-domain reference consistency | Subject parent keys, arm-code subsets, visit / epoch / element declarations | "Check whether cross-domain references are right" |
| Pinnacle 21-aligned validation | P21-compatible rule set with FDA / PMDA / NMPA severity and readiness scoring | "Run it the Pinnacle 21 way" |
| Define-XML self-check | Definition metadata reconciled against datasets | "Validate my define.xml" |
| Custom validation rules | Enterprise rule pack, isomorphic to built-in rules, self-checked before save | "Add our internal data-quality rules" |
| Auto-fix | Auto-fixable issues produce a corrected file with a per-change log | "Fix what you can automatically and give it to me" |

### ③ Specifications & configuration

| Capability | Description | Try saying |
|:---|:---|:---|
| Mapping-spec validation | Validate the spec before mapping; issues pinpointed to variables | "Check my mapping spec for problems first" |
| Spec Excel round-trip | Export / import specs, with consistency checks against standard domain references | "Our spec is maintained in Excel — round-trip?" |
| Study-config pre-check | Intercept illegal domain codes, out-of-bound variable names, missing required keys at config time | "Validate the SDTM naming and structure of my study config" |

### ④ Reverse CRF field matching

| Capability | Description | Try saying |
|:---|:---|:---|
| CRF field → SDTM variable | Multi-level matching on field name / label / sample values; recommends domain + variable + confidence, with evidence chains | "Which SDTM variable should CRF field 'Item 12' map to?" |

### ⑤ Export & submission

| Capability | Description | Try saying |
|:---|:---|:---|
| XPT v5 export | Submission-ready transport format per SAS XPORT V5 | "Export submission-ready XPT v5" |
| Define-XML v2.1 | Submission metadata source (bilingual labels, codelists, method defs, value-level rules) | "Generate define.xml" |
| Multi-sheet Excel report | Summary / per-domain detail / rule-hit statistics | "Give me an Excel check report" |
| Dataset version comparison | Column-level / row-level / value-level diff report | "What exactly differs between the two dataset versions?" |

### ⑥ Provenance & audit

| Capability | Description | Try saying |
|:---|:---|:---|
| Mapping provenance | Per-variable source column and rule ID | "Which column and which rule produced each variable?" |
| Audit trail | Tamper-evident audit records aligned with ALCOA+ principles | "I need an auditable trail" |
| Reproducibility self-check | Re-run the build and diff cell-by-cell against existing output | "Prove to an auditor that re-runs are identical" |
| Human adjudication list | Ambiguous terminology funneled to a single queue for human decision — the skill never decides for you | "Which terminology notices need a human call?" |

### ⑦ Reviewer's guides & test data

| Capability | Description | Try saying |
|:---|:---|:---|
| SDRG/ADRG guide drafts | PhUSE cSDRG v1.3 / ADRG v1.2 skeletons; unfillable items explicitly marked [TBC]; statistical-programmer review checklist attached | "Generate an SDRG draft from the PhUSE template" |
| Synthetic SDTM test data | Deterministic, reproducible, contains no real subject information; for validating downstream programs | "Generate structurally valid fake data to test my ADaM programs" |

---

## 2. Case Collection (conversation prompts · also available in the Workbench UI)

> Difficulty: ★ basic / ★★ advanced / ★★★ professional. Each card ends with a "🖥️ Workbench" pointer to the matching view. Note: on the web, mapping results are not persisted by default — the results panel *is* the artifact list; click "Download all as ZIP" to get the archive.

## ★ Basic usage

### Case 1 · One-shot mapping of a raw-data directory

**You say:**
> Map the clinical-trial raw data under data/raw into standard SDTM datasets.

**Assistant replies (sketch):**
> Read data/raw and identified 5 domains (DM / AE / LB / VS / EX); wrote sdtm_dm.csv (12 subjects), sdtm_ae.csv (38 adverse events), sdtm_lb.csv (156 rows), sdtm_vs.csv (84 rows), sdtm_ex.csv (60 rows). Three things happened automatically during mapping: ① value-level routing filled units (e.g., VSTESTCD=DIABP → VSSTRESU=mmHg); ② per-variable mapping provenance recorded (source column + rule ID); ③ compliance rule set ran, 2 issues total. No Errors — ready for downstream ct-adam.

**🖥️ Workbench:**
> Open the "Mapping" view → upload raw files (CSV/TSV) → pick "Target domain" on each file card → tick the domain grid → click "Map"; the results panel shows domain / rows / Issues and the artifact list.

### Case 2 · Preview before writing (safe preview)

**You say:**
> Don't write files yet — just preview how many rows each domain will map and how many issues there are.

**Assistant replies (sketch):**
> Preview mode (no files written): DM 12 rows / AE 38 rows (1 issue) / LB 156 rows (1 issue) / VS 84 rows / EX 60 rows. 350 rows total, 2 issues (both Errors). Preview and real mapping run the same code path, so row and issue counts match exactly. Say "start mapping" when you're ready.

**🖥️ Workbench:**
> "Preview mapping" in the "Mapping" view shows per-domain row estimates (nothing persisted); the real "Map" also doesn't persist — the results panel lists artifacts, and "Download all as ZIP" fetches the archive.

### Case 3 · Value-level mapping routing (auto unit fill)

**You say:**
> For vital signs I only have test codes and results, no unit column — can units be filled automatically?

**Assistant replies (sketch):**
> Value-level routing enabled: conditions in the built-in value-level rules drive routing — unique-value rules auto-fill, multi-value rules only prompt. This run: VSTESTCD=DIABP/SYSBP → VSSTRESU auto-filled mmHg (unique value); VSTESTCD=TEMP candidates are C/F (multi-value) → logged as a suggestion for human confirmation, not auto-filled. Two safeguards: placeholder value sets are never filled; result-type variables (original / standard / standard-numeric) never receive value-level fills.

**🖥️ Workbench:**
> Value-level routing and terminology validation are built into the mapping engine; tick "Controlled-terminology validation" and they take effect — no extra steps.

### Case 4 · Bilingual (Chinese/English) labeled datasets

**You say:**
> Give me a version with Chinese variable labels so non-technical colleagues can review it.

**Assistant replies (sketch):**
> Bilingual output enabled. Each SDTM dataset gains `<var>_LABEL_ZH` columns, e.g., USUBJID→Subject Unique Identifier, AETERM→Headache. Machine-initialized Chinese label coverage for common variables is about 87%; rare professional qualifiers can be filled in later. XPT variable labels use ASCII English (XPT v5 is an ASCII transport format; Chinese labels are used only in Define-XML bilingual fields).

**🖥️ Workbench:**
> "Bilingual label output" is ticked by default in the "Mapping" view; artifacts carry `_LABEL_ZH` columns.

### Case 5 · Custom column mapping (EDC adaptation)

**You say:**
> My EDC export column names differ — gender→性别, adv_event→不良事件; I dropped in a column-map file to map by it.

**Assistant replies (sketch):**
> Loaded your column-map file as a column-alias override: gender→SEX, adv_event→AETERM, ae_start→AESTDAT. Mapping complete with 0 required-field gaps; uncovered columns fall back to default rules. In the mapping provenance these variables are marked as coming from your spec (distinct from the built-in alias table), so you can check which columns came from your EDC spec.

**🖥️ Workbench:**
> The built-in alias table covers common EDC column names; fully custom names should go through the local full-featured edition with a mapping file.

## ★★ Advanced usage

### Case 6 · Controlled-terminology validation with near-miss suggestions

**You say:**
> While mapping, also run controlled-terminology checks and list any SEX, AESEV, RACE values that don't conform.

**Assistant replies (sketch):**
> Controlled-terminology validation complete. 3 violations found: AESEV="SEVRE" (closed codelist — did you mean 'SEVERE'?); AESEV="MODERAT" (did you mean 'MODERATE'?); RACE="Han" (extensible codelist — notice only, not blocking). Off-list values in closed codelists are Errors; new values in extensible codelists are notices — consistent with the extensibility rules. Empty values are not violations (missing values are handled by Required rules).

**🖥️ Workbench:**
> "Compliance Check" view → upload a dataset → pick a CT version → click "Terminology validation"; or use the "Controlled-terminology version" area at the bottom of the "Mapping" view to validate mapping output.

### Case 7 · CT version switching and multi-version reconciliation

**You say:**
> I want to confirm this data is compliant under the SDTMIG v3.4 (2022) terminology, and know whether conclusions change if I switch terminology versions.

**Assistant replies (sketch):**
> Active CT version: SDTMIG-3.4-2022. After switching versions and re-running, whether conclusions change depends on how each variable's values are classified in the old vs. new codelists. Unknown versions fall back to the top-level compatible terminology — no silent failure.

**🖥️ Workbench:**
> CT version switchers live in two places — the "Compliance Check" view and the "Controlled-terminology version" area of the "Mapping" view; switching re-runs validation and reports "version used / active version / variables validated".

### Case 8 · Custom validation rule pack (enterprise rules)

**You say:**
> Our company has internal data-quality requirements: USUBJID must not contain spaces, event terms must not exceed 200 characters, and AESTDTC is required when AESER=Y. Can these be added to validation?

**Assistant replies (sketch):**
> Written to the custom rules file (condition + params + severity; structure isomorphic to built-in rules). 4 new rules: USUBJID contains space → Warning; event term over 200 characters → Warning; AESER=Y missing AESTDTC → Error; term contains full-width punctuation → Notice. The pack is self-checked before save (ID conflicts / severity / condition / required params); if it fails, the engine refuses to enable it — no silent skipping. Emptying the pack disables the custom layer.

**🖥️ Workbench:**
> Custom rules are configured in the local full-featured edition; both "Mapping" and "Compliance Check" apply that rule layer — tick "CORE / CG.MODEL validation" in the Mapping view to enable validation.

### Case 9 · Reverse CRF field matching (evidence-chain version)

**You say:**
> A CRF field called "Item 12", also labeled "Item 12", with sample values M/F/U — match it to the most suitable SDTM variable.

**Assistant replies (sketch):**
> Reverse match result: recommend DM.SEX, confidence high / 0.98. Evidence chain: value-set evidence (3/3 values ⊆ SEX small closed codelist, 4 items total) + variable necessity (target variable Core=Required). Neither the field name nor the label gives a hint — the decisive match came purely from **value-set evidence**. Batch matching of multiple fields is also available: value-set evidence, form-structure coverage voting (e.g., when SVSTDTC is present, VISITNUM prefers SV), declarative rules (extensible), and mapping QA (date format / terminology value / sequence uniqueness) all apply.

**🖥️ Workbench:**
> Open the "Reverse Match" view; each line takes `field name,label[,sample value 1;sample value 2]`, click "Match"; results include recommended domain / variable / confidence and the evidence chain.

### Case 10 · Validate the mapping spec before mapping

**You say:**
> Check my mapping spec for problems first; if it's fine, then map.

**Assistant replies (sketch):**
> Mapping-spec validation complete: all 12 mappings point to legal SDTM variables; 1 notice — AE's ae_grade maps to AESEV but source values contain "Grade 3" text; preprocess to controlled terminology {MILD,MODERATE,SEVERE,NA} is recommended. Spec usable; continued to mapping with 0 Errors, 1 Warning final.

**🖥️ Workbench:**
> "Mapping Spec" view → upload a spec Excel → "Import and consistency check" runs the spec ↔ domain-reference consistency check; or click "Check built-in specs" to inspect the skill's built-in specs.

### Case 11 · Study-configuration pre-check

**You say:**
> Our protocol is just finalized — check the SDTM naming and structure of my study config first.

**Assistant replies (sketch):**
> Study-config check complete (fully local, zero network): [Error] domain "PAT" is not a standard SDTM domain code; [Error] variable "STUDYD" exceeds 8 characters; [Warning] domain VS is missing required KEY variable VISIT. 3 findings in total; issue list written.

**🖥️ Workbench:**
> No dedicated web view yet; run via the local full-featured edition (planned for the "Mapping Spec" view).

### Case 12 · Dataset version comparison (pre/post mapping, version diffs)

**You say:**
> I revised the mapping rules and re-ran — what exactly differs between the old and new datasets?

**Assistant replies (sketch):**
> Dataset comparison complete: row key auto-inferred as USUBJID + AESEQ. Result: rows 2 → 2; 1 row added (S1-003), 1 row removed (S1-002); columns +1 (AESTDTC); 1 value change (S1-001|1 AESER: N → Y). Output is a Markdown diff report (overview + column-level diff + value-level detail).

**🖥️ Workbench:**
> Open the "Dataset Compare" view; upload the old / new SDTM datasets (row key optional) → click "Run comparison"; results separate column / row / value-level differences.

## ★★★ Professional usage

### Case 13 · Full compliance validation (incl. model-level rules)

**You say:**
> Run a full SDTM compliance validation, focusing on model-level general rules — e.g., Findings domains misusing the study-day family.

**Assistant replies (sketch):**
> Full validation executed (model-level general rules + CORE rule set + cross-domain references): model rule 001 [Error] LB row 40 has LBSTDY (study-day variables forbidden in Findings); model rule 002 [Pass] no occurrence flags found in AE; CORE rules hit 6 (4 severity tiers: Reject / Error / Warning / Notice). Totals: Error 8 / Warning 23 / Notice 2.

**🖥️ Workbench:**
> "Compliance Check" view → upload converted SDTM datasets → click "Compliance check"; the results panel lists all issues (severity-colored), and clicking any row drills into the full record.

### Case 14 · Cross-domain reference consistency validation

**You say:**
> Check cross-domain references — e.g., USUBJID parent keys, and whether TA arm codes all fall inside DM's ARMCD.

**Assistant replies (sketch):**
> Cross-domain reference validation (15 rules): ① [Pass] all domains' USUBJID have parent records in DM; ② [Error] TA.ARMCD="ARM_C" not in DM.ARMCD; ③ [Warning] VS.VISITNUM="99" not declared in TV; ④ [Warning] AE.EPOCH="UNKNOWN_EPOCH" not declared in TA; ⑤ [Error] RELREC.RDOMAIN="FAKE_DOMAIN" references a non-existent domain code; ⑥ [Pass] SUPPQUAL references complete; ⑦ [Error] LB.STUDYID="OTHER" inconsistent with DM; ⑧ [Warning] LB.ARMCD="ARM_Z" not in DM.ARMCD; ⑨ [Warning] VISIT descriptions for VISITNUM=1 inconsistent across domains; ⑩ [Notice] LB.EPOCH contains an epoch absent from TA; ⑪ [Warning] TA.TAETORD=3 maps to multiple ELEMENTs; ⑫ [Warning] RELREC.IDVARVAL="99" absent from the parent domain; ⑬ [Warning] SUPPQUAL has duplicate (USUBJID, IDVARVAL, QNAM); ⑭ [Notice] DM.ARMCD=ARM_B has no TA counterpart; ⑮ [Warning] VS.VISITNUM="99" not declared in TV. The 15 rules cover subject parent keys / inter-domain enumeration references / relationship-domain and SUPPQUAL reference integrity / STUDYID global consistency / ARMCD global subset / VISIT description consistency / EPOCH enumeration consistency / TV coverage completeness / TA structural integrity.

**🖥️ Workbench:**
> Cross-domain references are part of the "Compliance Check" view; upload multiple domains (DM + TA + TV + child domains + RELREC) and all rules run together.

### Case 15 · Submission-ready XPT v5 + Define-XML v2.1 export

**You say:**
> Export the results as submission-ready XPT v5 and Define-XML v2.1 for downstream ADaM and eCTD consumption.

**Assistant replies (sketch):**
> Generated: sdtm_dm.xpt / sdtm_ae.xpt (XPT v5, conforming to SAS XPORT V5, directly readable by SAS / R / SPSS) and define.xml (Define-XML v2.1: ItemGroupDef/ItemDef/CodeList/ValueListDef/WhereClauseDef/MethodDef, with bilingual labels). Artifacts verified by cross-read (1–40 variables round-trip identical) — submission-ready.

**🖥️ Workbench:**
> Tick "XPT v5 export" in the "Mapping" view; artifacts include sdtm_<domain>.xpt and define.xml (fetched together via "Download all as ZIP").

### Case 16 · Pinnacle 21-aligned rule validation

**You say:**
> Our auditors run everything through Pinnacle 21 — can you align with its conventions?

**Assistant replies (sketch):**
> Yes. P21-compatible rule validation executed: ① SDTM dataset rules (three-agency severity: FDA / PMDA / NMPA); ② Define-XML metadata self-check (definition vs. dataset reconciliation, with a known-deviation register); ③ five rule tracks registered uniformly (built-in / CORE / custom / dataset rules / definition rules side by side), every hit carrying a rule ID and severity, with per-dataset readiness scoring.

**🖥️ Workbench:**
> The "Compliance Check" view outputs the unified rule set; Define-XML self-check runs alongside define.xml export.

### Case 17 · SDRG/ADRG reviewer's-guide drafts

**You say:**
> We need to submit an SDRG before submission — can you generate a draft from the PhUSE template?

**Assistant replies (sketch):**
> Draft generated per the PhUSE cSDRG v1.3 / ADRG v1.2 section skeletons (Chinese / English / bilingual): consistency checks, methodology, and change-history sections are auto-filled from define.xml and the datasets; anything that cannot be filled is explicitly marked [TBC] and funneled into the review checklist — nothing is fabricated; a statistical-programmer review checklist is attached (TBC items / rule alerts / consistency / missing, four sections). Positioned as a **draft framework** — never final, never a substitute for statistical-programming review.

**🖥️ Workbench:**
> The "Reviewer's Guide" page takes define.xml (ADRG additionally needs the ADaM define.xml) plus optional P21 report / protocol / SAP.

### Case 18 · Synthetic SDTM test data

**You say:**
> I want structurally valid fake data to validate downstream ADaM programs first — not real data.

**Assistant replies (sketch):**
> Synthetic SDTM data generated from your study config: same config + same seed = byte-identical output (reproducible); human confirmation required before anything is written; domain structure, controlled-terminology values, and cross-domain references all follow SDTMIG v3.4, so downstream programs can be validated directly; contains no real subject information.

**🖥️ Workbench:**
> Synthetic data generation currently runs via the local full-featured edition.

### Case 19 · Controlled-terminology human adjudication list

**You say:**
> Terminology validation reported a batch of notices — which ones need a human decision?

**Assistant replies (sketch):**
> Adjudication list generated: extensible-codelist new values and dictionary-less variables are funneled into a single queue, each awaiting your decision (approve / reject / map to a standard term) — the skill never decides ambiguous items for you, preventing uncertain values from being silently passed through.

**🖥️ Workbench:**
> The terminology-validation results area of the "Compliance Check" view.

### Case 20 · Compliance check and auto-fix of converted SDTM datasets

**You say:**
> I have an SDTM dataset someone else converted — run a compliance check and list non-compliant items in detail; auto-fix what you can.

**Assistant replies (sketch):**
> Compliance check executed (CORE rule set + model-level rules + cross-domain references + column order + value-level + controlled terminology): Error 8 / Warning 23 / Notice 2. On the web, clicking any issue row **drills into** the full field values of that record with the problem variable highlighted. Auto-fixable items produced a corrected dataset (domain code completion, date normalization, Chinese-value terminology mapping, with a per-change before/after log); structural issues like duplicate keys get fix suggestions but need human confirmation.

**🖥️ Workbench:**
> "Compliance Check" view → upload the dataset → click "Compliance check" → click an issue row to drill in → click "Download corrected CSV".

### Case 21 · Mapping provenance and full-chain traceability

**You say:**
> I need every mapping traceable: which column this variable came from, which rule it used — output an auditable trail.

**Assistant replies (sketch):**
> Mapping provenance generated (one file per domain): per-variable source column and rule ID — engine-generated (STUDYID / DOMAIN / USUBJID / --SEQ), spec-alias hits, built-in-alias hits, value-level routing fills, engine derivations (--DY / EPOCH / --STRESN), and no-source empty columns. Together with the tamper-evident audit trail and the run manifest (engine version + spec version + generation time), this forms a complete audit trail aligned with ALCOA+ principles.

**🖥️ Workbench:**
> "Download all as ZIP" in the "Mapping" results panel bundles the mapping provenance, define.xml, run manifest, and other traceability artifacts.

---

## 3. First-Time FAQ

**Q: I have no mapping spec — just EDC exports. Can it still map?**
A: Yes. A built-in alias table recognizes common EDC column names; for larger differences, provide a column-map file to override.

**Q: What raw-data formats are supported?**
A: CSV / TSV / Excel / SAS datasets / XPT / JSON are all supported, with per-domain auto-detection and normalization. Web uploads currently read CSV/TSV text — for other formats, convert to CSV locally first or use the local full-featured edition.

**Q: Do preview and real mapping agree?**
A: Yes — they run the same code path, so row and issue counts are identical; the only difference is that preview writes no files.

**Q: Once mapping finishes, how do I get the generated files?**
A: Locally, artifacts are written directly to your chosen output directory. On the web, the "Mapping" results panel *is* the artifact list — per-domain datasets, issue list, definition files, and provenance files are all there; click "Download all as ZIP" to fetch the archive.

**Q: Can mapping or validation results be adjusted to our conventions?**
A: Two layers: ① a column-map file overrides the "raw column → SDTM variable" correspondence; ② a custom rule pack adds/removes enterprise validation rules (self-checked before save; not enabled if it fails).

**Q: Is the output Chinese in a Chinese environment?**
A: Conversation language follows your question; datasets can carry Chinese variable-label columns and Define-XML supports bilingual labels. Note that the XPT transport format only supports ASCII English labels — Chinese labels live in Define-XML bilingual fields.

**Q: What if my data must stay confidential?**
A: Use the **local full-featured edition** — zero network, no egress; controlled terminology is bundled locally and raw data plus all artifacts stay on your machine.

**Q: Can the output be used directly for regulatory submission?**
A: XPT v5 + Define-XML v2.1 are generated to submission standards, but must still pass your QA and statistical-programming review before submission; SDRG/ADRG outputs are draft frameworks (unfillable items explicitly marked [TBC]) — never final, never a substitute for review.

**Q: What if I find an error in the results?**
A: Contact the author directly: medstatstar@gmail.com (Wintone Zhang / 张文彤).

---

## Output Artifacts

| File | Contents |
|---|---|
| `sdtm_<domain>.csv` | Compliant SDTM dataset per domain (e.g., `sdtm_dm.csv`), directly usable as `ct-adam` upstream input |
| `sdtm_issues.csv` | Consolidated issue list (rule IDs + severity Reject/Error/Warning/Notice) |
| `sdtm_supp_<domain>.csv` | SUPPQUAL supplemental qualifiers auto-routed when non-standard columns exist |
| `sdtm_<domain>.xpt` | XPT v5 dataset on XPT export, conforming to SAS XPORT V5, directly readable by SAS / R / SPSS, submission-ready |
| `define.xml` + `define_source_meta.json` | Define-XML v2.1 source (bilingual labels, codelists, method defs, value-level rules) |
| `<VAR>_LABEL_ZH` columns | Chinese variable-label columns added on bilingual output |
| `sdtm_<domain>.map_provenance.json` | Per-variable mapping provenance (source column + rule ID) |
| `run_manifest` | Run manifest (engine version + spec version + generation time; audit-trail equivalent) |
| `audit_trail.json` | Tamper-evident audit trail aligned with CDE Notice No. 44 ALCOA+ principles |
| `report.xlsx` | Multi-sheet Excel report (summary / per domain / rule statistics) |
| `mapping_spec.xlsx` | Mapping-spec Excel (export / import / consistency check, Case 10) |
| `diff.md` | Markdown diff report from dataset version comparison (Case 12: column / row / value level) |
| Custom rules file | Enterprise custom validation rule pack (Case 8) |
| Adjudication list | Terminology ambiguity queue for human decision (Case 19) |
| SDRG/ADRG draft | Reviewer's-guide draft (Chinese / English / bilingual, Case 17) |

---

## Version Baseline & Security Model (Tier B, highest priority)

- **Version baseline**: mapping rules and controlled terminology always follow **SDTMIG v3.4** + **Define-XML v2.1** — do not carry over v3.2.
- **Local processing**: raw / spec / artifacts all stay local; no reading of private user files without explicit authorization.
- **Zero network**: `network: none` by default; controlled terminology is bundled locally, never fetched online.
- **Traceable**: every mapping artifact carries rule IDs + spec version + engine version; the run manifest is the audit-trail equivalent (aligned with CDE 2026 Notice No. 44 ALCOA+ principles).

## Data & Privacy

| Usage mode | Where data goes | Notes |
|:---|:---|:---|
| Local full-featured edition (conversation prompts) | **No egress** | Raw data, mapping spec, and all artifacts are processed locally; zero network by default; controlled terminology is bundled locally |
| Shared online edition (web workbench) | **Sent to the Coze server** | Uploaded data must be sent to the server for processing; all users share a daily free quota; please evaluate and control the risk yourself |

---

**Version**: v2.4.0 | **License**: MIT | **Authors**: medstatstar, phoe-zip

For feature requests, bug reports, or other feedback, please contact the author directly at medstatstar@gmail.com (Wintone Zhang / 张文彤).

---

## Confidentiality Notice

> The CT series consists of 20+ specialized domain skills, organized into **two tiers — A, B** — by "whether the input contains confidential information" (see ct-base §11), providing full coverage of the entire new-drug clinical trial lifecycle.
>
> - **Tier A (non-confidential input)**: run fully locally using only ordinary data, or perform public retrieval; involves no confidential information. Tier A skills are published openly on GitHub.
> - **Tier B (confidential input)**: accept strictly confidential clinical-trial data / protocols / CRFs from pharma sponsors (e.g., ct-sdtm, ct-protocol, ct-eligibility); Tier B can be processed locally (no egress) or perform public retrieval (only public query terms leave the boundary), but packages are **not publicly released**; confidential input never ships with the package or leaves the machine. For custom / on-prem deployment, contact the author.
