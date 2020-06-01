# Pocket SNPs effects on Binding Affinity Prediction

This repository is part of the "Pocket SNPs effect on protein-ligand Binding Affinity Prediction (PSBAP)" project and it is the main repository to reproduce the project methodology and results.

**NOTE**: all the following instructions are for Linux operating system and tested on Ubuntu 18. These tools are not tested on other systems like Windows or MacOS.

Clone this repository to the location of your preference, then follow the next sections!



### First, build the Docker images for the required tools

Follow the instructions inside each one of the following repositories:

1. PSBAP Core:  https://github.com/ammar257ammar/psbap-core
2. PSBAP FoldX: https://github.com/ammar257ammar/psbap-foldx
3. PSBAP Gromacs: https://github.com/ammar257ammar/psbap-gromacs
4. PSBAP OpenBabel: https://github.com/ammar257ammar/psbap-openbabel
5. PSBAP Vina: https://github.com/ammar257ammar/psbap-vina

### Second, download the required datasets to the corresponding locations

1. Download ChEMBL database version 25 from the URL and unpack it to "data/chembl/chembl_25.sdf" :

   [ftp://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBLdb/releases/chembl_25/chembl_25.sdf.gz](ftp://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBLdb/releases/chembl_25/chembl_25.sdf.gz) 

2. Download UniProt natural variants database from the URL and unpack it to "data/uniprot_variation/homo_sapiens_variation.txt":

   [ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/variants/homo_sapiens_variation.txt.gz](ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/variants/homo_sapiens_variation.txt.gz) 

3. Register an account on [www.pdbbind-cn.org](www.pdbbind-cn.org) and login to download CASF2016 from the URL and unpack it to "data/pdbbind/CASF2016/coreset":

   http://www.pdbbind-cn.org/download/CASF-2016.tar.gz

   The folders of the PDB entries should be immediately under the mentioned path.

4. Define the following environment variables in the terminal:

   The path to the clone repository "pocket-snps-effect-binding-affinity" will be called PSBAP_ROOT for the remaining of this Readme file.

   ```bash
   export CONFIG_PATH=PSBAP_ROOT/config
   export DATA_PATH=PSBAP_ROOT/data
   export TSV_PATH=PSBAP_ROOT/tsv
   export FEATURES_PATH=PSBAP_ROOT/features
   export PROCESSING_PATH=PSBAP_ROOT/processing
   ```

   

### Third, start applying the PSBAP steps as following:

1. Filter CASF to include structure with quality <= 2.5 Angstrom, generate and download their (SIFTS, FASTA and DSSP), and map UniProt variant to the selected PDBbind protiens.

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op init
   ```

2. Map the selected UniProt variants to PDBbind proteins pockets, and prepare the folder structure for FoldX:

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op pocket-snps-mapping-and-foldx-prep
   ```

3. Run FoldX to introduce the mapped pocket's SNPs onto the proteins PDBs (choose NUM_OF_THREADS depending on the amount of CPUs you want to allocate for FoldX):

   ```bash
   docker run -it 	-v $PROCESSING_PATH/foldx:/pdb \
   				--name psbap-foldx --rm \
   				psbap-foldx RepairPDB NUM_OF_THREADS
   
   # After finish, run the next command:
   
   docker run -it 	-v $PROCESSING_PATH/foldx:/pdb \
   				--name psbap-foldx --rm \
   				psbap-foldx BuildModel NUM_OF_THREADS
   ```

4. Generate FoldX report:

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op foldx-report
   ```

5. Perform energy minimization on the proteins structures:

   ```bash
   docker run -it 	-v $PROCESSING_PATH/foldx:/pdb \
   				--name psbap-gromacs --rm \
   				psbap-gromacs prepare NUM_OF_THREADS true
   
   # After finish, run the next command:
   
   docker run -it 	-v $PROCESSING_PATH/foldx:/pdb \
   				--name psbap-gromacs --rm \
   				psbap-gromacs em NUM_OF_THREADS true
   				
   # After finish, run the next command:
   
   docker run -it 	-v $PROCESSING_PATH/foldx:/pdb \
   				--name psbap-gromacs --rm \
   				psbap-gromacs export NUM_OF_THREADS true
   ```

6. Prepare ligands folders for the corresponding selected PDBbind entries:

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op prepare-ligands-folders
   ```

7. Select similar ligands, split, perform energy minimization (MMFF94) using OpenBabel:

   ```bash
   docker run 	-v $PROCESSING_PATH/ligands:/pdb \
   			   -v $DATA_PATH/chembl:/index \
               --name psbap-openbabel --rm \
   			psbap-openbabel search-and-split
   
   # After finish, run the next command:
   
   docker run 	-v $PROCESSING_PATH/ligands:/pdb \
   			   -v $DATA_PATH/chembl:/index \
               --name psbap-openbabel --rm \
   			psbap-openbabel minimize
   ```

8. Prepare ligands information (IDs, Tanimoto index):

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op ligands-tanimoto-dataset
   ```

9. Prepare folders and configurations for AutoDock Vina:

   ```bash
   docker run 	-v $CONFIG_PATH:/config \
   			   -v $PROCESSING_PATH:/processing \
               -v $DATA_PATH:/data \
               -v $TSV_PATH:/tsv \
               -v $FEATURES_PATH:/features \
               --name psbap-core --rm \
               psbap-core -op prepare-vina-folders-config
   ```

10. Perform docking using AutoDock Vina by running the following script inside a bash script file :

    ```bash
    #!/bin/bash
    
    cd $PROCESSING_PATH/vina-docking
    
    for PDB in *; do
    
    	docker run -it 	-v $PROCESSING_PATH/vina-docking:/pdb \
                        --name psbap-vina --rm \
                        psbap-vina NUM_OF_THREADS PDB				
    done
    ```

11. Extract Vina results and generate mutation, pocket and ligand features:

    ```bash
    docker run    -v $CONFIG_PATH:/config \
    			      -v $PROCESSING_PATH:/processing \
                  -v $DATA_PATH:/data \
                  -v $TSV_PATH:/tsv \
                  -v $FEATURES_PATH:/features \
                  --name psbap-core --rm \
                  psbap-core -op generate-features
    ```

    
