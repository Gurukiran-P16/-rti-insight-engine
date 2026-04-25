# RTI Insight Engine

**Elevator Pitch:** Unlocking India's 3M+ unstructured RTI PDFs with Databricks and GPT OSS 120B.

The RTI Insight Engine is a Databricks-native pipeline that ingests raw RTI PDFs and transforms them into a structured anomaly detection platform. It features automated PySpark OCR extraction, Z-score anomaly detection for corruption flagging, and a semantic search/RAG interface powered by GPT OSS 120B.

![Architecture Diagram](architecture/architecture.png)

## Tech Stack
* **Platform:** Databricks Community Edition
* **Storage:** Delta Lake
* **Processing:** Apache Spark (PySpark UDFs)
* **Ingestion:** Databricks Auto Loader (`cloudFiles`)
* **AI/LLM:** GPT OSS 120B (via Databricks Model Serving)
* **Vector Search:** FAISS
* **UI:** Gradio

## 🚀 How to Run the Demo (For Judges)

### Prerequisites
You will need a **Databricks Workspace** (Community Edition works perfectly). Ensure you have a cluster running (e.g., DBR 13.x ML).

### Execution Steps
1.  **Clone this Repository:**
    Clone or download this repository to your local machine, or import it directly into your Databricks Workspace via "Repos".
2.  **Run Notebook 01 (`01_setup_bronze.py`):**
    Open this notebook in your workspace and click **Run All**. This will automatically generate synthetic RTI PDFs, save them to DBFS, and use Auto Loader to stream them into the `rti_hackathon.bronze_raw_pdfs` Delta table.
3.  **Run Notebook 02 (`02_silver_ocr.py`):**
    Open this notebook and click **Run All**. This utilizes a PyMuPDF Spark UDF to run parallel OCR extraction, saving the results to `rti_hackathon.silver_ocr_text`.
4.  **Run Notebook 03 (`03_gold_anomaly.py`):**
    Open this notebook and click **Run All**. This calculates Z-score anomalies for budget deviations and saves the flagged contracts to `rti_hackathon.gold_anomaly_scored`. It also logs metrics to MLflow.
5.  **Run Notebook 04 (`04_faiss_index.py`):**
    Open this notebook and click **Run All**. This will generate vector embeddings using the deployed GPT OSS 120B model and build a FAISS index on DBFS.
6.  **Run Notebook 05 (`05_gradio_ui.py`):**
    Open this notebook and click **Run All**. Wait for the final cell to execute. It will generate a public URL (ending in `*.gradio.live`). Click that link to open the interactive UI.

### Demo Flow (What to click)
* **Semantic Search Tab:** Try querying "contracts related to road construction" to see the semantic retrieval in action.
* **Ask GPT OSS 120B Tab:** Ask "Which ministry had the most anomalies?" to test the RAG implementation.
* **Anomaly Dashboard Tab:** View the automatically flagged high-risk contracts based on the Z-score logic.
