# Land Record Intelligence Platform (LRIP)

> **Intelligent Land Record Digitization & Validation System**
> **AI Intelligence & Validation Layer for Government Land Departments**
> *DEMO / PILOT PROTOTYPE — NOT AN OFFICIAL GOVERNMENT SYSTEM*

---

## 1. Overview & Core Mission

The **Land Record Intelligence Platform (LRIP)** is an AI-powered public-sector digitization and validation system that converts legacy land records—scanned PDFs, handwritten registers, sale deeds, historical mutation orders, and cadastral maps—into verified, structured, searchable digital land records.

LRIP is architected strictly as an **AI intelligence and deterministic validation layer** that interfaces with state revenue infrastructure (e.g. State Land Records Management Systems / Bhulekh, Registration Departments, Cadastral GeoServer GIS), rather than replacing authoritative government systems.

---

## 2. The 11-Stage AI & Governance Pipeline

```
Scanned Legacy Document (PDF / JPG / TIFF)
                  │
                  ▼
       [1] Document Classifier
                  │
                  ▼
       [2] Quality Analyzer (0–100 Score)
                  │
                  ▼
       [3] Image Pre-processing & Deskew
                  │
                  ▼
       [4] Multilingual OCR & HTR (English + Hindi)
                  │
                  ▼
       [5] Layout & Cell Analysis
                  │
                  ▼
       [6] 14-Field Cadastral Extraction
                  │
                  ▼
       [7] Standard Unit Normalization (Hectares & Acres)
                  │
                  ▼
       [8] Deterministic Validation Engine
                  │
                  ▼
       [9] Duplicate & Conflict Detection
                  │
                  ▼
       [10] Multi-Factor Confidence Engine
                  │
                  ▼
       [11] Split-Screen Human Verification (Lekhpal)
                  │
                  ▼
       District Officer (SDM) Authorized Publishing & GIS
```

---

## 3. Demo User Accounts & Instant Role Switcher

The platform includes built-in demo credentials and an **Instant 1-Click Role Switcher** in the top navigation header:

| Role | Username | Password | Operational Authority |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin` | `admin123` | Full system control, rule settings, user management |
| **Operator** | `operator` | `operator123` | Document ingestion, batch upload, queue tracking |
| **Verifier (Lekhpal)** | `verifier` | `verifier123` | Split-screen AI review, bounding box inspection, field correction |
| **District Officer (SDM)** | `officer` | `officer123` | District analytics, official publishing authorization, data export |
| **Auditor** | `auditor` | `auditor123` | Tamper-evident immutable audit logs, hash verification |
| **API Client** | `apiclient` | `client123` | Secure machine-to-machine integration gateway |

---

## 4. Quick Start: Running the Prototype

### Option A: Local Development (Instant Zero-Dependency Setup)

#### 1. Backend (Python 3.11+ / FastAPI)
```bash
cd backend
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```
*Backend runs on `http://localhost:8000`. Interactive API Docs available at `http://localhost:8000/api/docs`.*

#### 2. Frontend (Next.js 14 / TypeScript / Tailwind CSS)
```bash
cd frontend
npm install
npm run dev
```
*Frontend runs on `http://localhost:3000`.*

---

### Option B: Docker Compose (Single Command)

```bash
docker compose up --build
```

---

## 5. End-to-End Acceptance Test Scenario

Follow this 10-step walkthrough to test the complete workflow:

1. **Open Portal**: Navigate to `http://localhost:3000`.
2. **Login / Role Switch**: Switch to **Operator** via the top role selector.
3. **Ingest Document**: Go to **Document Ingestion** (`/documents`), click **+ Ingest Document**, select `Land Register`, Village `Bhaisamau`, District `Lucknow`, and submit.
4. **Inspect 11-Stage Pipeline**: Click **Inspect Stepper** on the uploaded document (`/documents/[id]`) and click **Re-run AI Pipeline** to watch the animated 11-stage execution.
5. **Switch to Verifier**: Switch to **Verifier** role.
6. **Split-Screen Verification**: Open the **Split-Screen Verification Interface** (`/verification/[id]`). Click any field (e.g. `Owner Name` or `Khasra Number`) on the right to see the document bounding box highlight on the left canvas.
7. **Field Revision & Active Learning**: Click the edit icon on `Plot Area`, adjust value from `1.25` to `1.20 Hectares`, add a note, and save.
8. **Mark Verified**: Click **Mark Verified (सत्यापित)** -> Status transitions to `VERIFIED`.
9. **Switch to District Officer**: Switch to **District Officer** role, review the record dossier (`/records/[id]`), and click **Approve & Publish (स्वीकृत करें)**.
10. **Cadastral GIS & Audit Trail**:
    - View the newly verified parcel in the **Cadastral GIS Map** (`/gis`).
    - View the tamper-evident log in the **Audit Trail** (`/audit`) and click **Verify** to validate the SHA-256 cryptographic signature.

---

## 6. Pre-Seeded Demonstration Datasets

- **50+ Cadastral Land Records**: Distributed across Lucknow (Bakshi Ka Talab, Mohanlalganj, Sadar) and Varanasi (Sadar, Pindra).
- **30+ Scanned Documents**:
  - `UP_LKO_BKT_Bhaisamau_Khasra_124_3_Clean.jpg` (Clean printed register, 96% confidence)
  - `UP_LKO_BKT_Kathwara_Hindi_Handwritten_Reg.jpg` (Hindi handwritten record, 84% confidence)
  - `UP_LKO_MHL_Poor_Scan_Degraded.jpg` (Degraded scan, 41/100 quality score)
  - `UP_LKO_BKT_Conflict_Mismatch_Area.jpg` (Historical area mismatch conflict)
  - `UP_LKO_BKT_Duplicate_Candidate_Scan.jpg` (88% duplicate collision candidate)
- **Interactive Cadastral GIS GeoJSON Parcels**: Real village centroids in Uttar Pradesh.
- **Active Learning Dataset**: Preserves human verifier corrections for continuous model benchmark fine-tuning.
- **Government Connectors**: Simulated State LRMS (Bhulekh), Cadastral GIS, and Registration Gateway.

---

## 7. AI & Optical Model Architecture

- **Image Quality Engine**: Laplacian variance sharpness, skew estimator, brightness/contrast histograms, and adaptive OTSU enhancement.
- **OCR / HTR Engine**: PaddleOCR + TrOCR handwritten model pipeline for English and Devanagari Hindi.
- **LLM / Vision Provider Layer**: Modular `LLMProvider` abstraction supporting `MockProvider` (deterministic zero-dependency evaluation), `OpenAIProvider` (`gpt-4o`), and `AnthropicProvider` (`claude-3-5-sonnet`).

---

## 8. Compliance & Legal Advisory

> [!NOTE]
> AI-generated extraction and confidence scores are advisory tools to assist revenue officers. In accordance with State Land Revenue Acts, the AI model does not possess independent legal authority to alter cadastral boundaries or ownership rights without authorized human officer review and digital signature.
