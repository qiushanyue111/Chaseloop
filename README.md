> [!IMPORTANT]
> ## 📁 Submission Documents & Templates
> **All required submission templates are in the Google Drive folder below. Download them before you start.**
>
> ### 👉 [**OPEN THE GOOGLE DRIVE FOLDER**](https://drive.google.com/drive/folders/1xw7c2aN8N-at8hPEULzkcU76e_FFb0Ef) 👈

---

# Project Name

> A one-sentence description of what your project does and who it is for.

---

## Team Information

| Field                  | Details                                     |
| ---------------------- | ------------------------------------------- |
| **Team Name**          | GO!GO!GO!                                   |
| **Team Domain**        | Domain 2                                    |
| **SDG Track (1–17)**   |  SDG 8 — Decent work and economic growth    |
| **Team Leader**        | PENG YUERAN                                 |
| **University**         | Xiamen University Malaysia                  |
| **Number of Members**  | 5                                           |

## Links

| Resource          | Link                                           |
| ----------------- | ---------------------------------------------- |
| GitHub Repository | https://github.com/your-username/your-project  |
| Replit Project    | https://9831efda-e953-4268-92a1-62dea10ab54e-00-eknsbighg0r1.pike.replit.dev/dashboard |
| Live Demo         | https://drive.google.com/file/d/1Bd4erCIiXz1h7iYf-Tpzq7MfT_gkqsSV/view?usp=sharing                |
| Slide Deck        | https://drive.google.com/file/d/14pGaXk3Qf0IhDcVcQt6kFbB30qT8E48q/view?usp=sharing                     |
| Demo Video        | https://youtu.be/s_TaRmwjVHE?si=rYzpxQQ86vm8T4C2              |

---

## Overview

### Problem

A Malaysian micro-SME finishes a job and then waits for the last 20–30% of the contract — the tail payment. The client already has the goods, the deposit is spent, and the owner's leverage is gone. Malaysian SMEs pay trade debts an average of 64 days late (Experian Malaysia, State of Credit 2025), and the tail lands last, exactly when cash is thinnest: the same ringgit is needed for the next job's materials and wages. In our 12 owner interviews, 8 of 12 chase final invoices personally on WhatsApp, 41% of tail invoices sat more than 30 days overdue, and owners spent a median of 9 hours a week chasing — mostly after 8pm.
### Solution

ChaseLoop runs one complete loop, not one message: an invoice crosses its due date (trigger), the agent assembles everything it knows about that client from the ledger, the chat history and the signed contract (connected data), a deterministic state machine and a penalty calculator decide what should happen today while an LLM writes the message in the client's own language (decision), the owner approves it from a card on his own phone (human approval), and the outcome — sent, promised, paid, disputed — is written to an append-only log that closes the case (visible outcome).

Two lines stay firm. No arithmetic is done by the model: dates, overdue days, penalty amounts and instalment splits come from a deterministic calculator that reads the contract, so every figure on screen traces to a clause. No consequential message leaves without a human tap: above a ringgit threshold, for any discount, and for any legal escalation, the send waits for approval.

### Key Features

- **Feature 1** — Trigger engine — an invoice is opened when its due date passes, or when a promise-to-pay date goes by without payment. Nothing is sent on a blind schedule.
- **Feature 2** — Case file builder — Excel/CSV ledger, WhatsApp and email thread history, and quotation PDFs are merged into one case file per invoice: amount, client, due date, terms, payment behaviour, dispute signals.
- **Feature 3** — Deterministic decision core — the state machine due → reminded → promised → escalated → settled plus a term-and-penalty calculator set every date and every ringgit. The LLM writes prose; it never computes money.
- **Feature 4** — Approval card on WhatsApp — "Chase Sdn Bhd, RM 4,380, 41 days late — send?" arrives on the owner's phone; one reply approves, edits or skips. No dashboard to log into at 9pm.
- **Feature 5** — Reply understanding — code-switched, typo-ridden replies (English / Bahasa Malaysia / Mandarin / Manglish) are parsed into a promise-to-pay (amount + date), a dispute, or a part-payment, and the next action is scheduled automatically.
- **Feature 5** — Reconciler and audit log — bank transfers are matched back to invoices, the case closes itself, and every touch, draft, approval and outcome is appended to an activity log that exports to the Orbyte hub.
### Tech Stack

| Layer         | Technology                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------- |
| Frontend      | Jinja2 templates + HTMX, server-rendered owner dashboard                                    |
| Backend       | Python 3.12, FastAPI, APScheduler (D-1 + every-3-day sweep); WhatsApp Cloud API for reminders |
| Database      | Replit PostgreSQL (Neon) for ledger and message log; SQLite for tests                       |
| AI / Compute  | DeepSeek API (deepseek-chat) — drafting EN/BM/ZH, reply-intent classification, screenshot fields |
| Hosting / Dev | Replit (Repl today → Deployments before judging) + Replit Secrets; GitHub                   |

---

## Project Structure

.
├── docs/                    # Screenshots, diagrams
├── runpod/
│   └── handler.py           # RunPod serverless handler (GPU workload)
├── src/
│   ├── main.py              # Application entry point
│   └── utils.py             # Helper functions
├── submission/              # Required submission documents
│   ├── SDC2026_TeamName_Preliminary.pdf      # Preliminary submission
│   ├── SDC2026_TeamName_TechDeclaration.pdf  # Technology declaration
│   ├── SDC2026_TeamName_Replit_POU.pdf       # Replit proof of use
│   └── SDC2026_TeamName_Runpod_POU.pdf       # RunPod proof of use
├── tests/
│   └── test_utils.py        # Unit tests
├── .env.example             # Example environment variables
├── .gitignore
├── .replit                  # Replit run configuration
├── LICENSE
├── requirements.txt         # Python dependencies
└── README.md


### Architecture
TAILPAY / PROJECT DEVELOPMENT                                  01 / 03

From idea to working prototype
Development journey reconstructed from project discussions.
Phases are grouped for clarity, not presented as a dated engineering log.

╔════════════════════════════════════════════════════════════════════╗
║ START                                                              ║
║ From an unpaid balance to a demonstrable prototype                 ║
╚══════════════════════════════════╦═════════════════════════════════╝
                                   ▼

┌─ 01  Identify the problem ─────────────────────────────── ref A+B ─┐
│ Finished work has been delivered, but final balances remain        │
│ unpaid. Owners repeatedly text or call customers.                  │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │
                                   ▼

┌─ 02  Choose users and use cases ───────────────────────────────────┐
│ Start with Malaysian micro-SME event services and custom-order     │
│ businesses. Owners and assigned staff use the workspace.           │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │
                                   ▼

┌─ 03  Agree the MVP scope ──────────────────────────────── ref A+B ─┐
│ Manual bill entry first. CSV import second. Add a bill when its    │
│ final amount and due date are known.                               │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │ 
                                   ▼

┌─ 04  Build the first working version ────────────────────── ref C ─┐
│ Create the web UI, API, database, sign-in and workspace            │
│ permissions. Track bills, balances, payments and reminders.        │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │
                                   ▼

┌─ 05  Review with the project team ─────────────────────────────────┐
│ Try the workflow, identify manual effort and missing actions, then │
│ revise sending, replies, preview access and bill cleanup.          │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │
                                   ▼

┌─ 06  Verify the changed behavior ────────────────────────── ref C ─┐
│ Check types/builds and relevant APIs. Test roles, retained payment │
│ data and critical owner/customer actions.                          │
└──────────────────────────────────┬─────────────────────────────────┘

                                   │
                                   │  
                                   ▼

┌─ 07  Deliver the current prototype ──────────────────────── ref D ─┐
│ Use the existing preview to demonstrate bills, simulated           │
│ reminders, customer replies, owner review and safe                 │
│ deletion/archive.                                                  │
└────────────────────────────────────────────────────────────────────┘
                                   ↳ 只在预览中演示，不对外发布

MAJOR REVISIONS: PAGE 2
REPEATABLE CHANGE LOOP: PAGE 3

Scope: development process from initial need to the current project output.      TAILPAY | 1


---

## How to Run

### Option 1 — Run on Replit (Recommended)

1. Open the [Replit Project](https://replit.com/@your-username/your-project), or import this repository via **Create Repl → Import from GitHub**.
2. Open the **Secrets** tab (🔒) and add the variables listed in `.env.example`:
   - `RUNPOD_API_KEY`
   - `RUNPOD_ENDPOINT_ID`
3. Click **Run**. Replit uses the `.replit` file to start `python src/main.py`.

### Option 2 — Run Locally

```bash
# 1. Clone the repository
git clone https://github.com/your-username/your-project.git
cd your-project

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
cp .env.example .env            # then fill in your values

# 5. Run the app
python src/main.py
```

### Run Tests

```bash
python -m unittest discover tests
```

---

## How We Use Replit *(Required)*

Describe how your team used Replit. For example:

- **Development:** Collaborative coding in a shared Repl using Multiplayer.
- **Hosting:** The app is deployed with Replit Deployments at the Live Demo link above.
- **Secrets:** API keys are stored in Replit Secrets, never in the code.

📄 Proof of use: [`submission/SDC2026_TeamName_Replit_POU.pdf`](submission/SDC2026_TeamName_Replit_POU.pdf)

## How We Use RunPod *(Required)*

Describe how your team used RunPod. For example:

We haven't used RunPod.
- **Workload:** Model inference (e.g. text generation, image processing) runs on a RunPod GPU.
- **Setup:** `runpod/handler.py` is deployed as a **RunPod Serverless Endpoint**.
- **Integration:** The Replit app sends requests to the endpoint using `RUNPOD_API_KEY` and `RUNPOD_ENDPOINT_ID`.

Example request:

```bash
curl -X POST https://api.runpod.ai/v2/$RUNPOD_ENDPOINT_ID/runsync \
  -H "Authorization: Bearer $RUNPOD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"input": {"prompt": "Hello"}}'
```

📄 Proof of use: [`submission/SDC2026_TeamName_Runpod_POU.pdf`](submission/SDC2026_TeamName_Runpod_POU.pdf)

---

## Challenges & Learnings

- What challenges did you face, and how did you overcome them?
- What did your team learn?

The model kept inventing penalties. Our first drafts cited late-payment charges that were not in the contract. We did not "prompt harder" — we moved every number out of the model into decisions.py, and let the LLM only write sentences around figures it is handed. This single change is why the proposal claims auditable maths.
WhatsApp template approval is a schedule risk. Meta reviews templates before they can be sent. We designed the loop so the channel is swappable (email fallback behind the same approval card) and the state machine never assumes a specific channel.
Promises are buried in Manglish. "boleh bayar separuh bulan depan" is a promise with a date, not chatter. We hand-labelled replies from our 12 interviews and treat promise extraction as a first-class task, because a promise is what closes the gap between "reminded" and "paid".
We had no Orbyte sandbox. So we simulated identity, RBAC and the activity log to the shape the hub describes, and kept the connector surface minimal (customer, invoice, task) — documented as an assumption rather than hidden.
Cold starts lie to demos. A 20-second GPU wake-up looks like a broken product. Warm-up ping plus in-process fallback model fixed it.

## Future Improvements

- [ ] Replace CSV import with connectors to Malaysian accounting tools (SQL Account, Bukku, Financio) so no one exports a spreadsheet at all.
- [ ] Per-debtor promise-keeping score, shared across Orbyte tenants only with explicit consent.
- [ ] Industry template library (renovation, events, printing) approved once per vertical to shorten WhatsApp review.
- [ ] On-premise / BYO-key inference for SMEs that will not send contract text to any cloud.
Team
- [ ] Voice-note understanding — many owners and clients send audio, not text, and it is currently ignored.

## Team

| Name        | Role                | GitHub                                   |
| ----------- | ------------------- | ---------------------------------------- |
| Peng Yueran | Team Lead / Backend | https://github.com/qiushanyue111 |
| Xu Xilin    | Frontend / Approval UX | https://github.com/yumubuzhixi |
| Li Zeshi    | AI                  | https://github.com/lizeshi-987star |
| Li Jiahao   | Data & Evaluation   | https://github.com/cst2609048 |
| Luo Yue     | Design & Documentation | https://github.com/Loyo20040925 |

---

## Submission Checklist

- [ ] Team name, domain (Domain 1 / Domain 2) and SDG track filled in
- [ ] Slide deck link added
- [ ] Demo video link added
- [ ] Replit used, with proof of use in `submission/`
- [ ] RunPod used, with proof of use in `submission/`
- [ ] Preliminary submission completed (`SDC2026_TeamName_Preliminary.pdf`)
- [ ] Technology declaration completed (`SDC2026_TeamName_TechDeclaration.pdf`)
- [ ] Submission files renamed with your team name (replace `TeamName`)
- [ ] "How to Run" steps verified
- [ ] No API keys or secrets committed to the repository

## License

This project is licensed under the [Apache License 2.0](LICENSE).
