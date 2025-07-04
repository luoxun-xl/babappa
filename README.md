# babappa

**BABAPPA**: **BA**sh-**B**ased **A**utomated **P**arallel **P**ositive selection **A**nalysis

**BABAPPA** is a fully automated, modular, and highly efficient pipeline designed for detecting episodic positive selection across gene families.
It integrates quality control, codon-aware multiple sequence alignment, phylogenetic reconstruction, branch and branch-site model testing using codeml (PAML), and statistical validation including likelihood ratio tests and Benjamini–Hochberg corrections.

A user-friendly GUI is also available for beginners.

## FEATURES

- End-to-end automation: from raw FASTA files to final positive selection reports
- Codon-aware MSA using PRANK
- Maximum-likelihood tree inference using IQ-TREE2
- Foreground branch generation for branch-site model testing
- Parallel execution of codeml jobs using GNU Parallel
- Automatic Likelihood Ratio Test (LRT) calculation and FDR correction
- Detailed final output in Excel (.xlsx) format

## REPOSITORY STRUCTURE

- `seqQC.py`: Quality control for input coding sequences
- `script0.sh`: Sequence QC + codon-based MSA (PRANK)
- `script1.sh`: Phylogenetic inference (IQ-TREE2) + foreground branch generation
- `4GroundBranchGenerator.py`: Create Newick files with labeled foreground branches
- `run_codeml.py`: Automated codeml model running (site, branch, branch-site)
- `script2.sh`, `script3.sh`: Parallelized codeml execution scripts
- `script5.sh`, `script6.sh`: Extraction of lnL and np values from codeml output
- `lrt_bh_correction.py`: LRT computation and Benjamini–Hochberg FDR correction (branch models)
- `lrt_bh_correction.sitemodel.py`: LRT and FDR correction for site models
- `script7.sh`, `script8.sh`: Batch LRT processing and final report generation
- `babappa.sh`: Main master script to run the full pipeline

## INSTALLATION INSTRUCTIONS

1. Create environment

```bash
conda create --name BABAPPA
conda activate BABAPPA

mamba install conda-forge::python=3.10
mamba install -c bioconda prank iqtree -y
mamba install -c conda-forge biopython parallel scipy statsmodels pandas openpyxl -y

# Or create environment from environment.yml
conda env create -f environment.yml -n BABAPPA
```

2. Install `paml.v4.9j`‘

- [Phylogenetic Analysis by Maximum Likelihood (PAML)](http://abacus.gene.ucl.ac.uk/software/paml.html#download)

```bash
cd ~/apps/biosoft
wget http://abacus.gene.ucl.ac.uk/software/paml4.9j.tgz

tar xf paml4.9j.tgz
rm bin/*.exe
cd src
make -f Makefile
ls -lF
rm *.o
mv baseml basemlg codeml pamp evolver yn00 chi2 ../bin
cd ..
ls -lF bin

bin/baseml
bin/codeml
bin/evolver

export PATH=$PATH:~/apps/biosoft/paml4.9j/bin/
```

3. Clone BABAPPA repository

```bash
git clone git@github.com:luoxun-xl/babappa.git

cd babappa
chmod a+x ./*.sh
```

## RUNNING BABAPPA

To run the full pipeline on your data:

```bash
./babappa.sh
```

## INPUT REQUIREMENTS

- Input FASTA files must contain coding sequences (CDS), `test.fasta`.
- Each sequence must:
  - Start with ATG (start codon)
  - End with a valid stop codon (TAA/TAG/TGA)
  - Be a multiple of 3 nucleotides
  - Have no internal stop codons
  - CDS length is divisible by 3

BABAPPA’s built-in `seqQC.py` script automatically filters and logs any problematic sequences.

## OUTPUT FILES

- MSA files (PRANK aligned)
- Treefiles (IQ-TREE maximum likelihood trees)
- Codeml outputs (for branch, branch-site, site models)
- Likelihood Ratio Test (LRT) results (Excel .xlsx files)
- Benjamini–Hochberg corrected p-values
- Summary reports highlighting genes under positive selection

----------------------------------------

## EXAMPLE FOLDER STRUCTURE AFTER RUNNING

```bash
BABAPPA/
├── QCseq/
├── msa/
├── treefiles/
├── foregroundbranch/
├── codemloutput/
├── codemlanalysis/
├── BHanalysis/
├── BHanalysis4sitemodel/
├── sitemodel/
├── sitemodelanalysis/
└── SiteModelBH/
```

## ACKNOWLEDGMENT

The name "BABAPPA" was lovingly inspired by my little son, whose fascination with butterflies led to "BABAPPA" becoming his joyful synonym for "butterfly."

## LICENSE

This project is licensed under the MIT License.

Happy Positive Selection Hunting with BABAPPA!