# T27 | IT Helpdesk Triage Bot

AI-assisted ticket classification, prompt iteration, and business impact analysis for the **T27 IT Helpdesk Triage Bot** mini project under the **Chatbots & AI Assistants** theme.

## Project Overview

The project builds a conversational IT helpdesk triage chatbot that reads a support ticket and produces:

- **Category**
- **Priority**
- **Short reason**
- **Approved first response from the Resolution Playbook**

The chatbot was developed in **Google AI Studio** and evaluated using a fixed 100-ticket benchmark. Model predictions were kept separate from the Gold Labels, and all accuracy verification was performed in Excel.

## Project Objectives

1. Build a working conversational helpdesk triage chatbot.
2. Evaluate prompt versions V1 to V4 using the same 100-ticket benchmark.
3. Verify predictions against the Gold Labels in Excel.
4. Improve the prompt using observed error patterns.
5. Estimate agent time saved.
6. Use the full 500-ticket dataset to derive business insights and make a manager-facing recommendation.

## Dataset

The project uses a synthetic teaching dataset containing:

- **500 support tickets**
- **7 ticket-level columns**
- **6 ticket categories**
- **4 priority levels**
- **6 approved Resolution Playbook responses**

### Fixed Evaluation Benchmark

The formal benchmark is:

`TCK00001` to `TCK00100`

The same 100 tickets were used for V1, V2, V3, and V4.

### Ticket Categories

- Billing
- Network/Connectivity
- Account/Login
- Hardware/Device
- Plan Change
- Cancellation

### Priority Levels

- Critical
- High
- Medium
- Low

## AI Data Separation

The chatbot receives only the information required for classification:

- Ticket ID
- Customer tier
- Channel
- Ticket text
- Resolution Playbook

The following remain outside the AI system:

- Gold Labels
- True category
- True priority
- Resolution outcome fields
- CSAT

Gold Labels are used only after prediction generation for Excel-based verification.

### Evaluation Flow

```text
AI-safe ticket data
        +
Resolution Playbook
        |
        v
Google AI Studio chatbot
        |
        v
Predictions for 100 tickets
        |
        v
Export predictions
        |
        v
Excel verification
        +
Gold Labels
        |
        v
Verified accuracy metrics
```

## Prompt Iteration

The prompt was improved through four documented versions.

| Version | Category Accuracy | Priority Accuracy | Both Correct | Decision |
|---|---:|---:|---:|---|
| V1 | 93% | 80% | 75% | Baseline |
| V2 | 100% | 80% | 80% | Kept |
| V3 | 100% | 81% | 81% | **Selected Final** |
| V4 | 93% | 76% | 71% | **Rejected** |

### V1

Established the baseline classification, priority, reasoning, and playbook-response instructions.

### V2

Introduced an explicit operational-impact and urgency calibration guide. The prompt clarified that urgency words alone should not force a Critical priority.

### V3

Added category boundary rules based on observed errors:

- Physical equipment faults should be classified as **Hardware/Device**, not Network/Connectivity.
- An explicit cancellation request should be classified as **Cancellation**, even when the ticket also contains a secondary service complaint.

V2's priority guidance was retained.

### V4

Added six worked examples and an output-validation checklist.

The additional examples did not improve the measured result. V4 regressed to 71% Both Correct accuracy and was therefore rejected.

**Final selected version: V3.**

## Final V3 Performance

V3 achieved:

- **100% category accuracy**
- **81% priority accuracy**
- **81% both-correct accuracy**
- **100% playbook compliance**

### Priority Performance

| Gold Priority | Accuracy |
|---|---:|
| Low | 100.0% |
| Medium | 100.0% |
| High | 72.9% |
| Critical | 40.0% |

The main weakness was priority under-classification. High-priority tickets were sometimes predicted as Low or Medium, and several Critical tickets were predicted as Low.

Because of this, the chatbot is recommended as an **agent-assist tool rather than an autonomous triage system**.

## Full 500-Ticket Business Analysis

The complete dataset was used for business analysis.

| Metric | Result |
|---|---:|
| Total tickets | 500 |
| Average resolution time | 15.07 hours |
| Average CSAT | 3.92 / 5 |
| Resolution time vs CSAT | r = -0.677 |

The negative relationship indicates that longer resolution times are associated with lower CSAT in this dataset. This is an observed association, not causal proof.

### Segment Insights

- **Business** customers have the lowest average resolution time at **13.19 hours** and the highest average CSAT at **4.05**.
- **Premium** customers average **15.28 hours** and **3.83 CSAT**.
- **Regular** customers average **15.34 hours** and **3.94 CSAT**.
- **App Chat** has the highest average resolution time at **15.40 hours**.
- **Twitter/X** has the lowest average CSAT at **3.81**.

## Agent Time-Saving Estimate

The time-saving calculation uses the project guide's illustrative assumptions:

- Manual triage: **3.0 min/ticket**
- Correct AI suggestion review: **1.0 min/ticket**
- Error correction: **2.0 min/ticket**
- Final V3 Both Correct accuracy: **81%**

### Estimated Impact

- AI-assisted triage: **1.38 min/ticket**
- Time saved: **1.62 min/ticket**
- Estimated monthly volume: **45.45 tickets**
- Estimated monthly saving: **1.23 hours**
- Estimated annual saving: **14.73 hours**

These are **scenario estimates**, not measured productivity results. The timing inputs should be replaced with observed agent timings in a real deployment study.

## Business Recommendation

### Recommended operating model

Use **V3 as an agent-assist chatbot**, not an autonomous triage system.

### Controls

- Human review should remain in place for priority decisions.
- **Critical tickets should receive mandatory human review.**
- Future prompt changes should be evaluated against the fixed 100-ticket benchmark.
- Resolution time and CSAT should be monitored after deployment to determine whether faster triage is associated with improved service outcomes.

The main reason for this recommendation is the gap between strong category classification and weaker priority classification, particularly the **40% accuracy on Critical tickets**.

## Responsible AI and Limitations

- The dataset is synthetic and intended for teaching.
- No real personal or confidential customer data was used.
- Gold Labels were not provided to the chatbot.
- AI predictions were treated as suggestions and verified against the answer key.
- Priority accuracy is not sufficient for autonomous routing.
- Time-saving estimates are based on assumptions rather than observed agent timings.
- V4 is retained as a documented failed iteration and was rejected as the final version.
- The 100-ticket benchmark is useful for regression testing, but an additional unseen test set would provide stronger evidence of generalisation.

## Verification Checklist

- [x] TCK00001-TCK00100 processed in every prompt run
- [x] 100 tickets present in every run
- [x] Predictions generated before Gold Label comparison
- [x] Excel verification completed for V1-V4
- [x] Playbook compliance verified
- [x] V3 selected based on measured results
- [x] V4 regression documented
- [x] Time-saving arithmetic reproduced
- [x] Full 500-ticket business analysis completed
- [x] AI use disclosed

## Tools Used

| Tool | Role |
|---|---|
| Google AI Studio / Gemini | Chatbot build and benchmark classification runs |
| ChatGPT | Planning, prompt drafting, error analysis, business analysis, and report structuring |
| Excel | Gold Label verification, accuracy analysis, error analysis, time-saving model, and insights |

## Project Evidence

### Google AI Studio

Build link:

https://aistudio.google.com/apps/b7b5377d-5199-4b32-897e-304d3b2ea6fc?showAssistant=true&showPreview=true

### Key Evidence

- Main chatbot interface
- 100-ticket benchmark processing
- V1-V4 prompt version history
- Excel-verified results
- Time-saving analysis
- Full 500-ticket business analysis
- Final recommendation

## Report

The detailed project report is included separately as:

`T27_IT_Helpdesk_Triage_Bot_Report.pdf`

## Author

**Madhumitha S R**  
Symbiosis Centre for Information Technology (SCIT), Pune  
Semester I | AI Powered Tools  
Task T27 | IT Helpdesk Triage Bot
