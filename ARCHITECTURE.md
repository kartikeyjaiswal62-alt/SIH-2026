# Technical Architecture Specification: Intelligent Land Record Digitization & Validation System (LRIP)

## 1. System Mission & Core Distinction

The **Land Record Intelligence Platform (LRIP)** operates as an **AI intelligence and deterministic validation layer** designed to interface with existing State Land Record Management Systems (LRMS / Bhulekh) and Cadastral GIS infrastructure.

```
+-----------------------------------------------------------------------------+
|                        GOVERNMENT DATA SOURCES                              |
|   Physical Scans & Register Books  |  State LRMS Portals  |  Cadastral Maps  |
+-----------------------------------------------------------------------------+
                                      │
                                      ▼
+-----------------------------------------------------------------------------+
|                      INGESTION & STORAGE ARCHITECTURE                       |
|   FastAPI Multi-Format Ingestion  |  S3-MinIO Compatible Document Storage   |
+-----------------------------------------------------------------------------+
                                      │
                                      ▼
+-----------------------------------------------------------------------------+
|                       11-STAGE AI & VALIDATION PIPELINE                      |
|                                                                             |
|  [01] Document Classification ──► [02] Image Quality Analysis (0-100)       |
|                                                     │                       |
|  [04] Multilingual OCR / HTR  ◄── [03] Image Preprocessing / Deskew         |
|             │                                                               |
|  [05] Layout & Table Analysis ──► [06] 14-Field Cadastral Extraction        |
|                                                     │                       |
|  [08] Deterministic Rules Engine ◄── [07] Standard Unit Normalization       |
|             │                                                               |
|  [09] Duplicate Detection     ──► [10] Multi-Factor Confidence Engine       |
|                                                     │                       |
|                                   [11] Verification Queue Routing           |
+-----------------------------------------------------------------------------+
                                      │
                                      ▼
+-----------------------------------------------------------------------------+
|                    SPLIT-SCREEN HUMAN-IN-THE-LOOP GOVERNANCE                |
|  Interactive Canvas Bounding Box Provenance | Lekhpal Field Revisions       |
+-----------------------------------------------------------------------------+
                                      │
                                      ▼
+-----------------------------------------------------------------------------+
|                  AUTHORITATIVE SDM APPROVAL & SPATIAL GIS                   |
|  District Officer Publishing  |  PostGIS Leaflet Map  |  SHA-256 Audit Trail|
+-----------------------------------------------------------------------------+
```

---

## 2. 11-Stage Pipeline Details

1. **Document Classification**: Hybrid classifier categorization into Land Register, Khasra, Khata, Mutation Record, Sale Deed, Cadastral Map, Historical Land Record, or Legal Paper.
2. **Quality Analyzer**: Computes Laplacian variance blur score, skew angle, brightness, contrast, noise, and composite readability score (0–100) with automatic recommendations.
3. **Image Pre-processing**: Perspective correction, deskew rotation, adaptive OTSU contrast stretching, and Gaussian denoising.
4. **Multilingual OCR & HTR**: Dual-engine pipeline supporting printed Hindi (Devanagari) and English, with fallback handwriting (TrOCR) processing.
5. **Layout & Table Analysis**: Geometric cell segmentation and bounding box association.
6. **Cadastral Entity Extraction**: Extracts 14 standardized attributes (Owner Name, Father Name, Co-Owners, Survey No, Khasra No, Khata No, Plot Area, Unit, Classification, Land Use, Mutation Order, Previous Owner).
7. **Unit Normalization**: Standardizes non-metric units (Pucca Bigha, Biswa, Katha, Sq. Ft) into Standard Hectares and Acres.
8. **Deterministic Validation Engine**: Deterministic Python rules verifying Khasra regex, positive area bounds, administrative hierarchy against master dictionary (Village -> Tehsil -> District -> State), and cross-document historical mismatches.
9. **Duplicate Detection Engine**: Fuzzy name similarity (Levenshtein) and spatial identifier collisions returning match probability percentages.
10. **Multi-Factor Confidence Engine**: Combines OCR confidence, quality scores, and validation penalties into a 0–100% composite score.
11. **Human Verification Queue**: Routes low-confidence or validation-flagged documents to the Verifier queue.

---

## 3. Data Model & Cryptographic Provenance

Every extracted field stores:
- `raw_value` and `normalized_value`
- `bounding_box`: `[ymin, xmin, ymax, xmax]` in normalized 0–1000 coordinate space
- `confidence`: float between 0.0 and 1.0
- `model_version`: e.g. `2.4.1`
- `source_document_name` and `page_number`
- `is_verified` and `is_corrected` flags with reviewer identifier and audit diff.

---

## 4. Security & Role-Based Access Control (RBAC)

| Role | Document Ingestion | Split-Screen Review | SDM Publishing | Audit Access | System Settings |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Administrator** | Yes | Yes | Yes | Yes | Yes |
| **Operator** | Yes | View Only | No | No | No |
| **Verifier (Lekhpal)** | No | Full Edit / Verify | No | View Logs | No |
| **District Officer (SDM)** | No | Yes | Full Approve / Export | Yes | No |
| **Auditor** | No | View Only | No | Full Audit / Verify | No |
