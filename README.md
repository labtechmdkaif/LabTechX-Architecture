# LabTechX-Architecture
Offline-first diagnostic &amp; pathology ERP architecture built with Electron.js, Node.js, SQLite.
# LabTechX — Production-Grade Offline-First Diagnostic ERP

LabTechX is an enterprise-grade, standalone diagnostic laboratory ERP designed for zero-latency clinical workflows, local persistence, and offline operability. Built using Electron.js, WebAssembly-based SQLite, and an embedded micro-service runtime, the system eliminates cloud roundtrips while providing comprehensive laboratory operations, commercial DRM, and hardware-locked compliance.



 Core System Architecture

The application is structured into four interconnected engine layers connected via an omnipresent event and IPC runtime:


+---------------------------------------------------------------------------------------+
|                                    PRESENTATION LAYER                                 |
|   Central Hub (index) | Patient Intake (newpatients) | Clinical Engine (reportmaker)  |
|   Financial Ledger (profit_collection) | B2B Dispatch & Inventory (stocks, b2blabs)   |
+---------------------------------------------------------------------------------------+
                                           │
                                 [Global Bridge Layer]
                               (global.js / window.FS)
                                           │
+---------------------------------------------------------------------------------------+
|                                   EXECUTION ENGINES                                   |
|  ┌─────────────────────┐  ┌─────────────────────┐  ┌───────────────────────────────┐  |
|  | LogicManager & Calc |  | Financial & Split   |  | Multi-Tier DRM Engine         |  |
|  | Dynamic Expressions |  | Anti-Double-Count   |  | AES-256 Stealth Persistence   |  |
|  | Abnormality Flags   |  | 7-Modality IP %     |  | Hardware Fingerprint (.ltx)   |  |
|  └─────────────────────┘  └─────────────────────┘  └───────────────────────────────┘  |
+---------------------------------------------------------------------------------------+
                                           │
                                  [IPC Secure Bridge]
                                (preload.js / main.js)
                                           │
+---------------------------------------------------------------------------------------+
|                               PERSISTENCE & SYSTEM CORE                               |
|  • SQLite WASM (sql-wasm.js) with physical write-back caching to AppData              |
|  • Local HTTP Sync Engine (Express.js / ShareFlowX)                                   |
|  • Tri-Tier Backup Rotation & Serialization (Archiver / JSZip)                         |
|  • Native Print Pipelines (Custom Canvas Pagination / OS Spoolers)                    |
+---------------------------------------------------------------------------------------+

⚙️ Key Architectural Highlights
1. In-Memory Wasm SQLite & Dynamic Schema Migration
Operates an in-memory SQLite database compiled via WebAssembly (sql-wasm.js) with synchronized binary write-back to the physical file on disk.
Self-healing database layer with automated PRAGMA checks and runtime non-destructive schema migrations (ALTER TABLE ... ADD COLUMN) to prevent data loss on version updates.
2. Clinical Computational Engine & Modular Designer
Domain-Specific Logic: In-line formula evaluation (LogicManager) allowing dynamic mathematical calculations (e.g., cross-analyte ratios, age/gender branching rules).
Automated Abnormality Engine: Real-time biological reference range evaluation with dynamic visual flags and conditional styling overrides.
Pixel-Perfect A4 Pagination: Custom multi-page height-budgeting algorithm (SAFE_BUFFER = 15px) preventing margin collapse and cleanly distributing dynamic headers, footers, and digital signatures.
3. Integrated Commercial DRM & Hardware Security
Hardware-Locked Machine ID: Generates deterministic hardware footprints using Motherboard Serial, BIOS Serial, CPU ID, and UUID via WMI queries.
Stealth Store Anti-Tamper: Employs obfuscated persistence (stealthStore.js) in AppData using AES-256-CBC ciphering with fake system headers, ensuring license and terms validity even under local database resets.
Proprietary File Association: Custom .ltx encrypted license file format registered at the OS level, handled via Electron single-instance listeners.
4. Financial Audit & 7-Modality Incentive Engine
Anti-Double-Counting Ledger: Intelligent reconciliation separating direct collections, due recovery cycles, and daily operating expenses with Cash vs. UPI payment splits.
Granular Incentive Partitioning: Modality-wise percentage engine managing commissions across 7 streams: Pathology, X-Ray, USG, CT, MRI, ECG, and Miscellaneous.
Integrated B2B Outsourcing: Live dispatch tracking tied directly into the automated operating expense ledger.
5. Automated Disaster Recovery & Diagnostics
Tri-Tier Backup Rotation: Automatic point-in-time raw snapshots, manual ZIP bundle archives, and scheduled external drive sync with a 7-backup FIFO rotation policy.
Automated Crash Interceptor: Global error telemetry catching unhandled JavaScript promises and DOM errors, with automatic rotated dumps in local diagnostic logs.
🛠️ Technology Stack
Layer
Technologies & Libraries
Desktop Runtime
Electron.js (v42+), Node.js, Express.js
Data Engine
SQLite via WebAssembly (sql-wasm), JSON Data Stores
Frontend Foundation
HTML5, CSS3, Modern JavaScript (ES6+), WebSockets
Document & Rendering
jsPDF, html2canvas, Pica (Lanczos Resizer), PDF.js, XLSX, QRGen
Archival & Storage
Archiver, JSZip, Local Storage Caching
Distribution
Electron-Builder (Windows NSIS x64, macOS DMG, Linux AppImage)

📂 Primary Functional Workstations
Clinical Intake & Billing: Automated incremental Lab IDs, smart demographic parsing, split payment settlement, and thermal/A5 slip generation.
Reporting Studio: Visual canvas builder (GraphicsEngine), formula parser, encrypted report exports (.ltxrpt), and batch status toggles.
Accounts & Audit Desk: Real-time net collection matrices, expense ledgers, payable/receivable badging, and automated audit trails.
Procurement & Inventory: Supplier catalogs, multi-item purchase vouchers, side-by-side vendor rate comparison matrices, and batch/expiry auditing.
Governance & Security: Multi-tenant user base (Admin + User slots), granular parent-child permission masks (auth-element), and two-factor deletion guards with dynamic captchas.

---
