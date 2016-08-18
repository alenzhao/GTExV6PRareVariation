# GTExV6PRareVariation
Repository to reproduce analyses from the GTEx V6P Rare Variation Manuscript

# To run the code
Download processed files from \<website\>

# Pipeline
## Expression data correction and normalization

#### Make directory to hold PEER normalized data
mkdir /srv/scratch/restricted/goats/preprocessing/PEER

#### Generate read count matrices from GTEx V6P combined file
python preprocessing/split_by_tissues.py \ <br>
	--GTEX \<path to count data\>/GTEx_Analysis_v6p_RNA-seq_RNA-SeQCv1.1.8_gene_reads.gct \ <br>
    --SAMPLE \<path to processed_data directory\>/gtex_samples_tissues.txt \ <br>
    --OUT /srv/scratch/restricted/goats/preprocessing/PEER \ <br>
    --END .reads.txt

#### Process the RPKM matrices for V6P into correct format for the PEER pipeline
(depends on process.rpkms.v6p.py) <br>
bash preprocessing/process.rpkms.v6p.sh

#### Run bash scripts to generate PEER corrected data (includes non-EAs) with covariates removed
bash PEER/goats_peer_pipeline_covs.sh /srv/scratch/restricted/goats/preprocessing/PEER

#### Make list of individuals and tissues for flat file creation
bash preprocessing/get_tissue_by_individual.sh

#### Make flat files from raw RPKMs and PEER-corrected data for all tissues and individuals
(Takes 10 minutes or so to run) <br>
python preprocessing/gather_filter_normalized_expression.py <br>
python preprocessing/gather_filter_rpkm.py