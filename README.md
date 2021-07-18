# PSnpBind, a database to highlight pocket SNPs effects on protein-ligand binding affinity

This repository is part of the "PSnpBind, a database to highlight pocket SNPs effects on protein-ligand binding affinity" project and it is the main repository to reproduce the project methodology and results.

**NOTE**: all the following instructions are for Linux operating system and tested on Ubuntu 18. These tools are not tested on other systems like Windows or MacOS.

Clone this repository to the location of your preference, then follow the next sections!



### First, build the Docker images for the required tools

Follow the instructions inside each one of the following repositories:

| Tool                                                         | More Info                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [PSBAP Core](https://github.com/ammar257ammar/psbap-core)    | [![Codacy Badge](https://app.codacy.com/project/badge/Grade/f88127b901bd40b48b9a3bab4b309703)](https://www.codacy.com/gh/ammar257ammar/psnpbind-core/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=ammar257ammar/psnpbind-core&amp;utm_campaign=Badge_Grade) ![GitHub top language](https://img.shields.io/github/languages/top/ammar257ammar/psnpbind-core) ![Lines of code](https://img.shields.io/tokei/lines/github/ammar257ammar/psnpbind-core) ![GitHub](https://img.shields.io/github/license/ammar257ammar/psnpbind-core) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/ammar257ammar/psnpbind-core) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/aammar/psnpbind-core) [![DOI](https://zenodo.org/badge/227237183.svg)](https://zenodo.org/badge/latestdoi/227237183) [![Dockerhub](https://img.shields.io/badge/Dockerhub-aammar%2Fpsnpbind--core-green)](https://hub.docker.com/r/aammar/psnpbind-core) [![Documentation](https://img.shields.io/badge/Documentation-Javadoc-blue)](https://ammar257ammar.github.io/psnpbind-core/) |
| [PSBAP FoldX](https://github.com/ammar257ammar/psbap-foldx)  | ![GitHub top language](https://img.shields.io/github/languages/top/ammar257ammar/psnpbind-foldx) ![GitHub](https://img.shields.io/github/license/ammar257ammar/psnpbind-foldx) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/ammar257ammar/psnpbind-foldx) [![Dockerhub](https://img.shields.io/badge/Dockerhub-aammar%2Fpsnpbind--foldx-green)](https://hub.docker.com/r/aammar/psnpbind-foldx) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/aammar/psnpbind-foldx) [![DOI](https://zenodo.org/badge/224151495.svg)](https://zenodo.org/badge/latestdoi/224151495) |
| [PSBAP Gromacs](https://github.com/ammar257ammar/psbap-gromacs) | ![GitHub top language](https://img.shields.io/github/languages/top/ammar257ammar/psnpbind-gromacs) ![GitHub](https://img.shields.io/github/license/ammar257ammar/psnpbind-gromacs) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/ammar257ammar/psnpbind-gromacs) [![Dockerhub](https://img.shields.io/badge/Dockerhub-aammar%2Fpsnpbind--gromacs-green)](https://hub.docker.com/r/aammar/psnpbind-gromacs) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/aammar/psnpbind-gromacs) [![DOI](https://zenodo.org/badge/266883870.svg)](https://zenodo.org/badge/latestdoi/266883870) |
| [PSBAP OpenBabel](https://github.com/ammar257ammar/psbap-openbabel) | ![GitHub top language](https://img.shields.io/github/languages/top/ammar257ammar/psnpbind-openbabel) ![GitHub](https://img.shields.io/github/license/ammar257ammar/psnpbind-openbabel) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/ammar257ammar/psnpbind-openbabel) [![Dockerhub](https://img.shields.io/badge/Dockerhub-aammar%2Fpsnpbind--openbabel-green)](https://hub.docker.com/r/aammar/psnpbind-openbabel) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/aammar/psnpbind-openbabel) [![DOI](https://zenodo.org/badge/266906942.svg)](https://zenodo.org/badge/latestdoi/266906942) |
| [PSBAP Vina](https://github.com/ammar257ammar/psbap-vina)    | ![GitHub top language](https://img.shields.io/github/languages/top/ammar257ammar/psnpbind-vina) ![GitHub](https://img.shields.io/github/license/ammar257ammar/psnpbind-vina) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/ammar257ammar/psnpbind-vina) [![Dockerhub](https://img.shields.io/badge/Dockerhub-aammar%2Fpsnpbind--vina-green)](https://hub.docker.com/r/aammar/psnpbind-vina) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/aammar/psnpbind-vina) [![DOI](https://zenodo.org/badge/241072278.svg)](https://zenodo.org/badge/latestdoi/241072278) |



### Second, download the required datasets to the corresponding locations

1. Download ChEMBL database version 25 from the URL and unpack it to "data/chembl/chembl_25.sdf" :

   [ftp://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBLdb/releases/chembl_25/chembl_25.sdf.gz](ftp://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBLdb/releases/chembl_25/chembl_25.sdf.gz)

   

2. Download UniProt natural variants database from the URL and unpack it to "data/uniprot_variation/homo_sapiens_variation.txt":

   [ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/variants/homo_sapiens_variation.txt.gz](ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/variants/homo_sapiens_variation.txt.gz) 

   

3. Register an account on [http://www.pdbbind-cn.org](http://www.pdbbind-cn.org) and login to download CASF2016 from the URL and unpack it to "data/pdbbind/CASF2016/coreset":

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

11. Extract Vina docking results:

    ```bash
    docker run    -v $CONFIG_PATH:/config \
    			  -v $PROCESSING_PATH:/processing \
                  -v $DATA_PATH:/data \
                  -v $TSV_PATH:/tsv \
                  -v $FEATURES_PATH:/features \
                  --name psbap-core --rm \
                  psbap-core -op generate-dockings-results
    ```

    
