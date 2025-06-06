# 🧬 ONT-Amplicon-Workflow-MEG

Welcome to the ONT Amplicon Analysis Workflow for the Microbiome Ecology Group (MEG, Ákos T. Kovács Lab)! 🎉  
This workflow is designed to help you analyze Oxford Nanopore Technologies (ONT) amplicon sequencing data in a reproducible, beginner-friendly way — directly on the IBL BLIS server. 🖥️🐧

---

## 🧪 What is this?

This is a practical, streamlined workflow to go from raw ONT reads to **taxonomic assignment**.  
In this example, we show how to process **elongation factor Tu (tuf)** gene amplicons, but the same approach can be extended to other marker genes (e.g., 16S full length, V3V4 region).

---

## 👩‍🔬 Who is this for?

This pipeline is meant for:
- Members of the Ákos T. Kovács Lab 🧫
- Microbiologists new to bioinformatics 🤓
- Users working on the IBL **BLIS** server 🖧

No prior scripting knowledge is needed — just follow the folder structure and commands step-by-step!

---

## 🧰 What's included?

- 📂 Example dataset: ONT reads targeting **elongation factor Tu**
- 🔧 Custom reference database (FASTA) for BLAST
- 🧾 Taxonomic assignment scripts
- 🗃️ Sample outputs

---

## 🚀 How to run this workflow

1. **🔐 Log in to the BLIS server**  
   Open your terminal and connect to the BLIS server:

   ```bash
   ssh 132.229.120.188
🧑‍💻 Don’t have an account?
Please contact 👉 c.du@biology.leidenuniv.nl to request access.

📘 Make your admin happy!
Before using the server, read the IBL Bioinformatics Wiki — it contains essential guidelines and good practices. 💡

📚 Read the ONT-AmpSeq tutorial (recommended!)
This workflow is adapted from ONT-AmpSeq by Mathias Helmer Eskildsen 🧠.

We've customized it for:

The BLIS server setup 💻

The needs of the Microbiome Ecology Group (MEG) 🧬

🔧 Some steps may look different due to file paths, tools, or lab-specific choices — but the core logic remains the same.
