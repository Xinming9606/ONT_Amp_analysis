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
   ```

🧑‍💻 Don’t have an account?
Please contact 👉 c.du@biology.leidenuniv.nl to request access.

📘 Make your admin happy!
Before using the server, read the IBL Bioinformatics Wiki — it contains essential guidelines and good practices. 💡


2. **📚 Read the ONT-AmpSeq tutorial (recommended!)**  

  This workflow is adapted from [ONT-AmpSeq](https://github.com/MathiasEskildsen/ONT-AmpSeq) by Mathias Helmer Eskildsen 🧠.

  We've customized it for:
        - The BLIS server setup 💻
        - The needs of the Microbiome Ecology Group (MEG) 🧬
  
  🔧 Some steps may look different due to file paths, tools, or lab-specific choices — but the core logic remains the same.


3. **🧰 Install ONT-AmpSeq (Bash version)**  

⚠️ We do not use Snakemake here, as it's tricky to configure on the BLIS server.

✅ Instead, we recommend using the bash-script version of ONT-AmpSeq.

   ```bash
   cd /path/to/home-dir/ONT-AmpSeq-main
   micromamba env create -f ONT-AmpSeq_bash_version.yml
   ```

4. **🔍 Check your data quality with Nanoplot**  

If you want to know a bit more about [Nanoplot](https://github.com/wdecoster/NanoPlot)

  ```bash
  cd /path/to/home-dir/ONT-AmpSeq-main
  bash workflow/scripts/nanoplot.sh -t 1 -j 1 -o .test/stats_out -i .test/test_data
   ```
  You should see your read quality under the stats/ directory.

  📄 The most important file is: NanoPlot-report.html

  Based on the report, you can set these parameters for the next step: length_lower_limit=800, length_upper_limit=1200, Q-score=20 (for example)


5. **🧬 Prepare your BLAST database**  

In this case, we used amplicons from the elongation factor Tu gene.

📦 The BLAST database is available at [KovacsLab-BLASTdb](https://github.com/Xinming9606/KovacsLab-BLASTdb
)


6. **🚀 Actually run ONT-AmpSeq**  

If you're already using the Blis server, the ONT-AmpSeq pipeline environment has been installed — it's called ONT_Amp_bash.
If you're installing it in your own environment, the environment should be named OTUtable.


 ```bash
  micromamba activate ONT_Amp_bash
  bash workflow/scripts/ONT-AmpSeq_bash_version.sh \
    -t 4 -j 3 \
    -i .test/test_data \
    -o output_test \
    -l 1200 -u 1600 -q 20 \
    -r blastn \
    -d .test/databases/Elongation_factor_Tu
  ```
  🛠 Adjust the -l, -u, and -q parameters based on your NanoPlot results.

7. **⚠️ Issues with vsearch**

Sometimes vsearch may not generate results if your read quality is too low — this can lead to an empty 8_OTUtable folder. 😢

👉 Depending on your research goals:

If you do not need a perfect abundance plot and just want to check what taxa are present in your sample, you can use the concatenated OTU file in 5_otus_cat for a direct BLAST search. 🔍

For example:

```bash
blastn -query /path/to/home-dir/ONT-AmpSeq/sample_output/5_otus_cat/otus_concatenated.fa \
       -out /path/to/home-dir/ONT-AmpSeq/sample_output/blast_result.txt \
       -db Elongation_factor_Tu2 \
       -outfmt 10 \
       -max_target_seqs 1
```

✨ In this example, I used the concatenated OTU file to run a BLAST search against our Elongation Factor Tu database.


