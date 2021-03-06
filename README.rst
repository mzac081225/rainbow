RAINBOW - An open-source pipeline for rare genetic disease analysis
===================================================================

What is RainBow?
---------------
RainBow is an open-source pipeline to analyze rare disease patient genomes.
It aims to make a causal variant candidate report for a specific Mendelian 
disease, in order to promotie diagnosis by the patient's physician.

It is developed by Rare Genomics Institute Korea (or RG Korea. Visit 
http://www.raregenomics.or.kr for further information),
a Korean branch of the non-profit organization Rare Genomics Institute
(or RG. Visit http://www.raregenomics.org for further information).

RainBow consists of three parts:

1. Setup (setup.py)
Download required databases and softwares to local and install them.

2. Run short read processing pipeline (run_proc_pipeline.sh)
Process short reads and call variants.

3. Make causal variant report (make_report.py)
Filter variants and make causal variant candidate report.


Third party software/database license
-------------------------------------
RainBow contains files not developed by the Rare Genomics Institute Korea.
Files in the subdirectory bin/ and files downloaded by setup.py follow their
own licenses. We recommend you read them, since not all of them follow the same 
license as RainBow.

Among the third party softwares, Platypus maintains a mailing list to inform 
users of latest updates on Platypus. You can register to this mailing list by 
enlisting on the following URL from The Wellcome Trust Centre for Human 
Genetics: http://www.well.ox.ac.uk/software-download-registration


License
-------
RainBow follows The MIT License (MIT). This license applies to all parts of this
software except the third party software/databases documented in 
THIRD-PARTY-NOTICES.txt. 
For further information, we recommend you read our LICENSE.txt or the contents 
in https://opensource.org/licenses/MIT


How to use it?
--------------

1. Download files
Download required softwares and databases for running Rainbow.

    ./setup.sh

2. Install Platypus
Download and install Platypus (preferably v0.7.9.1). Then, move or copy the
Platypus directory to /path/to/rainbow/bin.
We will refer to "/path/to/rainbow" as rainbow_dir from now on.

    mv Platypus_X.X.X /path/to/rainbow/bin/

or,

    cp Platypus_X.X.X /path/to/rainbow/bin/


3. Link raw fastq/fastq.gz data
Rename or link the fastq files of your samples to /path/to/data/fastq/"ID"
We will refer to "/path/to/data" as data_dir from now on.
Here, "ID" should be the case ID of your sample, e.g. test
When analyzing paired-end samples, forward and reverse fastq files
should have exactly _1 and _2 noted before .fastq/.fastq.gz suffices.

Example:

    mkdir datadir/fastq
    mkdir datadir/fastq/test
    mv /path_to/SRR000000_1.fastq.gz data_dir/fastq/test/test_1.fastq.gz
    mv /path_to/SRR000000_2.fastq.gz data_dir/fastq/test/test_2.fastq.gz


4. Write ID and info files
For each case, one ID file should be created.
For each case or case ID, at least one sample ID should be written in
an ID file.
For example, If the case with the ID "test" includes three
samples (e.g. patient, mother, father), you should include all the 
sample IDs in the case ID file, with the case ID being the prefices.
Example ID files are in the info directory of "/path/to/rainbow", or from now
on, rainbow_dir.

Example:

Case ID for "test" case: test
Sample IDs for "test" case: "test_patient", "test_father", "test_mother".
Then, when

    cat rainbow_dir/info/id_test.txt  # the contents of the ID file printed

    test_patient
    test_father
    test_mother

should be printed, disregarding the order of the sample IDs.


5. Run read processing and variant calling pipeline
Your working directory is your data directory, where you have put your fastq 
files in. The working directory is the data_dir which has been mentioned above.
Also, do not wrongly input the end form of your sequencing samples (se for 
single-end, pe for paired-end).

Example:

    rainbow_dir/run_proc_pipeline.sh test pe rainbow_dir/info/id_test.txt


6. Make your analysis report
After finishing your case information file in (e.g. 
rainbow_dir/info/info_test.txt) run the make_report.py script.

Example:

    rainbow_dir/make_report.py rainbow_dir/info/info_test.txt

Then your report output will be in data_dir/var_flt/"ID"/*.result.txt


7. Running test sample
The following is a run example using the test data. Run the following scripts 
after installation and data download are complete.


8. Another run example
The following is a run example, after installation and data download are 
complete.

    cd /my/data
    mkdir /my/data/fastq
    mkdir /my/data/fastq/FAMILY1

    cat /my/data/id_FAMILY1.txt

        FAMILY1_father
        FAMILY1_patient

    rainbow_dir/run_proc_pipeline.sh FAMILY1 pe /my/data/id_FAMILY1.txt

    cat /my/data/info_FAMILY1.txt | grep -v "^#" # removing comment lines

        FAMILY1 # Case ID, or family ID. Let's call this CASE.ID for below

        FAMILY1_patient/FAMILY1_father/n # Sample IDs. Should be exact as CASE.ID_id.txt

        y/y/n # Is each sample affected? yes[y] or no[n]

        AD # Pedigree model

        1,2,3,4 # Common SNP filtering stringency

    rainbow_dir/make_report.py
    
