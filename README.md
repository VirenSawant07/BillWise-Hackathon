# BillWise — Bharat Builds Tour Hackathon

**Team VersionOne** · *First Commit* (Event 01) of the **Bharat Builds Tour** by **WeMakeDevs × AWS** · September 2026 · 🏆 Participant

BillWise lets you upload an Indian hospital or pharmacy bill and shows which charges have a government price ceiling (NPPA), and where each charge sits against it. Every result shows its arithmetic and the government order it comes from, and lines it can't check are marked grey instead of guessed.

🔗 **Source code:** https://github.com/Alternateacc-1/BillWise_Project
🌐 **Live app:** https://prod.d29dj39cdf0dkb.amplifyapp.com/

---

## 🙋 My contribution

- Built the complete frontend for **page one and page two** in React 19, Vite and Tailwind CSS
- The frontend is hosted on **AWS Amplify** and talks to the serverless backend (**API Gateway → AWS Lambda**)
- Found and fixed frontend glitches and rendering issues across the app

## 🏆 Certificate

<p align="center">
  <img src="bharatbuilds virencert.png" alt="Certificate of Participation — Viren Sawant, Bharat Builds Tour: First Commit, WeMakeDevs × AWS" width="750">
</p>

---

## 👥 Team VersionOne

| Member | Role |
|---|---|
| **Tejas Mogare** (Team Lead) | Supervised every part of the project; built the complete backend and integrated the services with the frontend |
| **Satvik Shinde** | All AWS services and their integration, the full pipeline including Amplify; a key part of the project idea |
| **Viren Sawant** | Complete frontend for page one and page two; fixed frontend glitches and rendering issues |
| **Shreyas Pakhale** | Error and bug fixes, managing records, and maintaining the full Git repository |

---

## ☁️ AWS architecture

```
  Browser (React 19 + Vite, hosted on AWS Amplify)
        │  bill photo or PDF
        ▼
  Amazon API Gateway ──► AWS Lambda (Python 3.12, FastAPI)
                            ├──► Amazon S3 ........ stores the uploaded bill
                            ├──► Amazon Textract ... reader 1: line items, quantities, totals
                            ├──► Amazon Bedrock .... reader 2: Amazon Nova via the Converse API
                            ├──► rule engine ....... plain Python, no model in the pricing decision
                            └──► Amazon DynamoDB ... stores the finished report
```

| Service | Role in BillWise |
|---|---|
| AWS Amplify | Hosts the React frontend |
| Amazon API Gateway | Receives the bill upload and passes it to Lambda |
| AWS Lambda | Runs the whole Python backend |
| Amazon S3 | Stores uploaded bills |
| Amazon DynamoDB | Stores finished reports |
| Amazon Textract | First reader: extracts line items from the bill |
| Amazon Bedrock (Nova) | Second, independent reader; if the two disagree, the line is marked grey |
| AWS SAM + boto3 | Whole stack defined in one template; Python SDK for all AWS calls |
| AWS Budgets | Cost control from day one |

### Key design decisions
- **Two readers that are meant to disagree.** With a single reader, 4 of 7 lines came back "high confidence" and produced a wrong finding, while Textract reported 96–99 % confidence. When the readers disagree, BillWise refuses to price the line.
- **A model-agnostic Converse API.** When the original model's Marketplace offer expired mid-project, switching to Amazon Nova was a one-line config change.
- **Infrastructure as code.** The SAM stack was torn down and rebuilt several times while sorting out Regions, and came back identically every time.
- **Data residency.** Bedrock geo inference profiles are pinned in IAM, so medical bills can't leave the chosen Regions.

---

*The source code belongs to Team VersionOne and lives in the [original repository](https://github.com/Alternateacc-1/BillWise_Project). This repository documents the project, the team, and my contribution.*
