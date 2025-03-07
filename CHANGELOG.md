### 0.9.7

There was some strange issue with the numbering, so bumped to 0.9.7

* Moved the arm selection function for fields in the chromosome name to the ```ChromosomeArm``` class.

### 0.8.7

* FEATURE: ```polymarker.rb``` now also prints the total number of hits found.

### 0.8.6

* BUGFIX: ```priemr3.rb``` had a regression when adding the repetitive flag to the ```@values``` array. This lead to the
  wrong order of the columns in the output and possibly other secondary effects.

### 0.8.5

* Added the option ```--max_hits``` to ```polyamarker.rb``` to set a maximum number of bast hits to identify repetitive
  regions. This adds the column ```is_repetitve``` to the output. The mask is not calculated in repetitive regions and
  the primers are designed as non-specific.

### 0.8.4

* Added script ```tag_stats.rb`` That gets the descriptive statistics for a tag in a bam file for each reference.

```bash
ruby tag_stats.rb -b HI.3206.006.Index_2.CS_125RNA_14d_Leaf8.sorted.bam -r /Users/ramirezr/Dropbox/JIC/expVIPMetadatas/RefSeq1.0/Genes/annotation/IWGSCv1.0_UTR_ALL.cdnas.fasta --tag 'NH'
```

### 0.8.3

* BUGFIX: ```ChromosomeArm.rb``` was fixed to conform the module assumptions for the package.

### 0.8.2

* FEATURE: The functions to select the chromosome arm are now in ```lib/bio/PolyploidTools/ChromosomeArm.rb``` and the
  help message is updated automatically with the valid options.
* FEATURE: Added option ```filter_best``` to replicate the original behaviour of selecting the best hit of each
  chromosome. Still useful for assemblies which still contain synthetic duplications.

### 0.8.1

* BUGFIX: There was an error which prevented the correct localisation of the SNP in markers with gaps in the local
  alignment before the position with the snp.
* FEATURE: PolyMarker now selects the best hit of the target chromosome. This improves the specificity in regions with a
  recent duplication. The drawback is that if your assembly has artificial repetitions, the primers won't be marked as '
  chromosome specific', but as 'chromosome semi-specific'. In a future version this will be addressed.

### 0.8

* FEATURE: ```polymarker.rb``` added the flag ```--aligner blast|exonerate ``` which lets you pick between ```blast```
  or ```exonerate``` as the aligner. For blast the default is to have the database with the same name as the
  ```--contigs``` file. However, it is possible to use a different name vua the option ```--database```.

### 0.7.3

* FEATURE: ```polymarker.rb``` Added to the flag ```--arm_selection``` the option ```scaffold```, which now supports a
  scaffold specific primer.
* FEATURE: ```snp_position_to_polymarker``` Added the option ```--mutant_list```  to prepare files for PolyMarker from
  files with the following columns ```ID,Allele_1,position,Allele_1,target_chromosome```.

### 0.7.2

* FEATURE: Added a flag ```min_identity``` to set the minimum identity to consider a hit. The default is 90

### 0.7.1

* BUGFIX: Now the parser for ```arm_selection_embl``` works with the mixture of contigs and pseudo-molecules
* DOC: Added documentation on how to use custom references.

### 0.7.0

* Added flag ```genomes_count``` for number of genomes, to be used on tetraploids, etc.

### 0.6.1

* polymarker.rb now validates that all the files exist.
* BUGFIX: A reference was required even when it was not used to generate contigs.
