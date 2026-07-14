# Azure Data Factory - Dynamic Pipeline Project

This repository contains an Azure Data Factory (ADF) project built with **parameterized, dynamic pipelines** for ingesting data from HTTP sources into an Azure Data Lake (Bronze layer).

## 📌 Overview

Instead of creating a separate dataset/pipeline for every file, this project uses **dynamic datasets** driven by parameters — so a single pipeline can ingest multiple files/folders just by changing parameter values at runtime.

**Flow:**
```
HTTP Source (raw.githubusercontent.com)
        │
        ▼
Dynamic Dataset (parameterized: URL, folder, file name)
        │
        ▼
Azure Data Lake Storage (Bronze container)
```

## 🗂️ Repository Structure

```
ADF/
├── dataset/
│   ├── ds_Git_Parameter.json     # Parameterized dataset for source config
│   ├── ds_Raw.json               # Static raw dataset (products.csv)
│   ├── ds_git_dynamic.json       # Dynamic dataset - source (parameterized URL)
│   ├── ds_http.json              # HTTP source dataset
│   └── ds_sink_dynamic.json      # Dynamic dataset - sink (parameterized folder/file)
├── factory/
│   └── azure-project-adf1.json   # Factory-level settings & managed identity config
├── linkedService/
│   ├── httplinkedservice.json    # Linked service - HTTP source
│   └── storagedatalake.json      # Linked service - Azure Data Lake (Bronze)
├── pipeline/
│   └── ...                       # Pipeline definitions
├── Data/
│   └── ...                       # Sample source files
└── publish_config.json           # ADF Git publish branch config
```

## ⚙️ Key Components

| Component | Purpose |
|---|---|
| `httplinkedservice` | Connects to public HTTP source (GitHub raw content) |
| `storagedatalake` | Connects to Azure Data Lake Storage (ADLS Gen2) |
| `ds_git_dynamic` | Source dataset — file path passed in via `p_rel_url` parameter |
| `ds_sink_dynamic` | Sink dataset — target folder/file passed in via `p_sink_folder` / `p_sink_file` parameters |

## 🔐 Security Notes

- Authentication for `storagedatalake` uses Azure Data Factory's **Managed Identity** — no plaintext credentials are stored in this repo.
- No secrets, keys, or connection strings are committed. Any credential-like fields are Azure-managed and encrypted at rest.

## 🚀 How It Works

1. Pipeline triggers with parameter values for source URL and target folder/file name
2. Data is pulled from the HTTP source using `ds_git_dynamic`
3. Data lands in the **Bronze** container of Azure Data Lake via `ds_sink_dynamic`
4. Because everything is parameterized, adding a new file to ingest requires no new dataset — just new parameter values

## 🛠️ Setup (to redeploy this factory)

1. Create an Azure Data Factory instance
2. Go to **Manage → Git configuration** and connect this repository
3. Set collaboration branch to `main` and root folder to `/ADF`
4. Publish from ADF Studio to deploy resources to your live factory

## 📄 License

This project is for personal/portfolio/learning purposes.
