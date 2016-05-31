# Ancestral-BLocks-Reconstruction project
## Sypnosis
This project provide ancestral reconstruction tools dedicated to bacteria genomes.

## Model Assumption
* Phylogenetic Tree
  1. Our phylogenetic tree is given, it is binary and rooted.
  2. Leaves are populated by orthoblocks. At least one leaf has a reference operon.
  3. The model is agnostic to gene order.
* Relationship between parent nodes and their children
  1. Given a parent gene blocks, its children gene blocks can't have any gene that is not in the parent gene blocks. (Hard assumption)
  2. There are 3 types of events that can happen from a parent to a child:
     * Split      : If two genes in one taxon are neighboring and their homologs in the other taxon are not, then that is defined as a single split event. The distance is the minimal number of split events identified between the compared genomes.
     * Deletion    : A gene exists in the operon in the one taxon, but its homolog cannot be found in an orthoblock in another taxon. Note that the definition of homolog, e-value 10−10 is strict, and may result in false negatives. The deletion distance is the number of deletion events identified between the compared target genomes.
     * Duplication : A duplication event is defined as having gene j in a gene block in the source genome, and homologous genes (j′,j″)(j′,j″) in the homologous block in the target genome. The duplication distance is the number of duplication events counted between the source and target genomes. The duplication has to occur in a gene block to be tallied.
  3. Multiple events from parent to children are possible.
  4. Events are treated as independent.

## Installation
User can either use github interface Download or type the following command in command line:
```bash
git clone https://github.com/nguyenngochuy91/Ancestral-Blocks-Reconstruction
```
User has to install python, and ete3 (would recommend use anaconda package). The instruction can be followed from this site:
http://etetoolkit.org/download/

## Usage

Cautious:
There are several different between running python2 or python3 (such as divide function
). My program is written in python3, if you want to run it correctly in python2, please uncooment the second line in the program "findparent.py".
Here is the step by step usage:
* Step 1: Parse each operon file in the result dic, and convert it into appropriate form:
  1. For each gene in an operon file, map it into an alphabet letter
  2. For each genomes, use the start, stop position and strand to find neighbor gene pairs, only display those genomes info that actually have at least orthoblocks
  3. Use the command line below and the output will be stored in directory new_result
```bash
./convert.py  -n result_dic/ -o new_result/ 
```
* Step 2: Using each operon file from new result, and the tree input (this tree was built using muscle alignment of the 33 taxa on the rpOb gene marker), then provide ancestral reconstruction that minimize edit distance between any parent and its 2 closest children:
1. Use the command line below and the output will be stored in directory reconstruction
```bash
./reconstruction.py -i new_result/ -t muscle.ph -o reconstruction/ 
```
* Step 3: Provide a visualization of the ancestral reconstruction process using ete3 package, it also provide a grouping theme depends on the class of the taxa
1. Use the command line below for each operon that you like, here I use a highly conserved operon rplKAJL-rpoBC:
```bash
./show_tree.py -o reconstruction/rplKAJL-rpoBC -g group.txt 
```
2. Use the command line below for each operon that you like, here I use not so conserved operon caiTABCDE:
```bash
./show_tree.py -o reconstruction/caiTABCDE -g group.txt 
```

## Credits
1. http://bioinformatics.oxfordjournals.org/content/early/2015/04/13/bioinformatics.btv128.full 



