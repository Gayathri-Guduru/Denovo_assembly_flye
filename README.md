# Denovo_assembly_flye

# Data Analysis
Programs required: it is recommended that the user has anaconda installed, through which all required programs can be installed. Assuming that anaconda is available, all the required programs can be installed using the following:

```
conda create -n flye -c bioconda -c conda-forge -c defaults flye
```
## Introduction
This pipeline is compatabile with reads generated by Oxford Nanopore or PacBio.

Before we run FastQC, you should be on a compute node in an interactive session. Please run the following srun command if you are not on a compute node. An interactive session is very useful to test tools and workflows.
```
srun --pty -t 0-6:00 --mem 5G /bin/bash
```

Now, after you are on interactive mode, develop the scripts required for analysis and submit the jobs using"
```
sbatch "filename.sh"
```

And, the squeue command is used to pull up information about jobs in the queue, by default this command will list the job ID, partition, username, job status, number of nodes, and name of nodes for all jobs queued or running within SLURM.
```
squeue -u username
```
And if you want to end the interactive mode type *exit*

# The files provided initially is ```raw_data.fq.gz```

# Inorder to start the analysis, we need to activate the conda environment 
```conda activate flye```

# Assembly using FLYE
# Create a script name ```flye.sh``` that has the command to perform assembly.
```
/home/guduru.g/miniconda3/envs/flye/bin/flye --nano-raw raw_data.fq -o out_flye -g 1m -t 10 -i 2
```

# outputs
00-assembly, 20-repeat, 30-contigger, 10-consensus, 40-polishing
params.json
assembly_graph.gv
assembly_graph.gfa
assembly.fasta
assembly_info.txt

# It also provides assembly stats

# Next step is mapping the reference genome to the assembly fasta file.
# Download the genbank sequence from https://www.addgene.org/50005/sequences/ and convert it to fasta format
 
 # create a ```genbank_to_fasta.py``` python script to convert genbank sequence to fasta format
```
import warnings
from Bio import BiopythonParserWarning
warnings.simplefilter('ignore', BiopythonParserWarning)
from Bio import SeqIO                                                                                                                                                                                             # Replace this with the path to your GenBank file
genbank_file_path = "/home/guduru.g/flye/addgene-plasmid-50005-sequence-222046.gbk"                                                                                                                               # Replace this with the desired output path for the FASTA file
fasta_file_path = "/home/guduru.g/flye/reference_pUC19.fasta"
# Converting GenBank file to FASTA format
with open(genbank_file_path) as input_handle, open(fasta_file_path, "w") as output_handle:
sequences = SeqIO.parse(input_handle, "genbank")
count = SeqIO.write(sequences, output_handle, "fasta")                                                                                                                                                            print(f"Converted {count} records to FASTA format")
```
