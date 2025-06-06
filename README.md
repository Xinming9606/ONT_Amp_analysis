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

2. **📚 Read the ONT-AmpSeq tutorial (recommended!)**
This workflow is adapted from ONT-AmpSeq by Mathias Helmer Eskildsen 🧠.
We've customized it for:
- The BLIS server setup 💻
- The needs of the Microbiome Ecology Group (MEG) 🧬
🔧 Some steps may look different due to file paths, tools, or lab-specific choices — but the core logic remains the same.

3. 🧰 Install ONT-AmpSeq (Bash version)

> ⚠️ We do **not** use Snakemake here, as it's tricky to configure on the BLIS server.  
> ✅ Instead, we recommend using the bash-script version of ONT-AmpSeq.

   ```bash
   cd /path/to/home-dir/ONT-AmpSeq-main
   micromamba env create -f ONT-AmpSeq_bash_version.yml

4. **📊 Check your data quality (important!)**
To inspect your sequencing reads quality, use the nanoplot.sh script:

cd /path/to/home-dir/ONT-AmpSeq-main
micromamba env create -f stats.yml
micromamba activate stats

```bash
bash workflow/scripts/nanoplot.sh \
  -t 1 \
  -j 1 \
  -o .test/stats_out \
  -i .test/test_data

📁 This will generate a report inside .test/stats_out/
📌 The key file is:

NanoPlot-report.html

Open this file in your browser to evaluate:

Average read quality (Q-score)

Length distribution of reads

💡 Based on this report, you can choose filtering thresholds for the next step.
In our case, we typically set:

length_lower_limit = 1200
length_upper_limit = 1600
Q-score threshold = 20

5. 🔎 Prepare your BLAST database
For this project, we use elongation factor Tu (EF-Tu) amplicons.

The curated BLAST database can be found at:

📦 KovacsLab-BLASTdb

Make sure to download the EF-Tu database and place it under:
.test/databases/Elongation_factor_Tu

6. 🧪 Run ONT-AmpSeq (bash version)
Activate the environment and run the main workflow script:

```bash
micromamba activate OTUtable

bash workflow/scripts/ONT-AmpSeq_bash_version.sh \
  -t 4 \
  -j 3 \
  -i .test/test_data \
  -o output_test \
  -l 1200 \
  -u 1600 \
  -q 20 \
  -r blastn \
  -d .test/databases/Elongation_factor_Tu

📌 Notes:

-l, -u, and -q should be adapted based on your NanoPlot-report.html results

-r blastn specifies the method used for taxonomic assignment

-d should point to your local copy of the EF-Tu BLAST database
