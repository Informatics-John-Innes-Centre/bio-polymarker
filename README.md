# bio-polymarker

Tool to design KASP primers, making them as specific as possible.

---

## Installation

Polymarker makes use of the following applications as such these need to be available on the system and in the users
```$PATH``` variable:

* [MAFFT](https://mafft.cbrc.jp/alignment/software/)
* [primer3](https://primer3.org/)
* [exonerate](https://github.com/cb2e6f/exonerate)
* [blast](https://blast.ncbi.nlm.nih.gov/doc/blast-help/downloadblastdata.html)

Fundamentally the steps for installation are:

* install required applications
* install polymarker dependencies
* `gem install bio-polymarker`

### RHEL / AlmaLinux

A more detailed example of installing polymarker, and it's required tools on a 
fresh install of RHEL/AlmaLinux 9 can be found [here](INSTALL.md).

---

## Running

To run PolyMarker with the CSS wheat contigs, you need to unzip the reference file
from  [ensembl](http://ftp.ensemblgenomes.org/pub/release-25/plants/fasta/triticum_aestivum/dna/Triticum_aestivum.IWGSC2.25.dna.genome.fa.gz).

```sh
polymarker.rb --contigs Triticum_aestivum.IWGSC2.25.dna.genome.fa --marker_list snp_list.csv --output output_folder
```

The ```snp_list``` file must follow the convention ```ID,Chromosome,SEQUENCE``` with the SNP inside the sequence in the
format [A/T]. As a reference, look at test/data/short_primer_design_test.csv

If you want to use the web interface, visit the [PolyMarker webservice at TGAC](http://polymarker.tgac.ac.uk)

The available command line arguments are:

```
Usage: polymarker.rb [options]
    -c, --contigs FILE               File with contigs to use as database
    -m, --marker_list FILE           File with the list of markers to search from
    -g, --genomes_count INT          Number of genomes (default 3, for hexaploid)
    -s, --snp_list FILE              File with the list of snps to search from, requires --reference to get the sequence using a position
    -t, --mutant_list FILE           File with the list of positions with mutation and the mutation line.
    requires --reference to get the sequence using a position
    -r, --reference FILE             Fasta file with the sequence for the markers (to complement --snp_list)
    -o, --output FOLDER              Output folder
    -e, --exonerate_model MODEL      Model to be used in exonerate to search for the contigs
    -i, --min_identity INT           Minimum identity to consider a hit (default 90)
    -a, --arm_selection arm_selection_embl|arm_selection_morex|arm_selection_first_two
                    Function to decide the chromome arm
    -p, --primer_3_preferences FILE  file with preferences to be sent to primer3
    -v, --variation_free_region INT  If present, avoid generating the common primer if there are homoeologous SNPs within the specified distance (not tested)
    -x, --extract_found_contigs      If present, save in a separate file the contigs with matches. Useful to debug.
    -P, --primers_to_order			 If present, saves a file named primers_to_order which contains the KASP tails
```

## Input formats

The following formats are used to define the marker sequences:

### Marker list

If the option ```--marker_list FILE``` is used, the SNP and the flanking sequence is included in the file. The format
contains 3 columns (the order is important):

* **snp_name** The ID of the marker. Must be unique.
* **target chromosome** for the specific primers. Must be in line with the chromosome selection criteria.
* **sequence** The sequence flanking the SNP with the SNP highlighted on square brackets (```[]```) and the two alleles
  separated by a forward slash (```/```).

#### Example:

```
BS00068396_51,2A,CGAAGCGATCCTACTACATTGCGTTCCTTTCCCACTCCCAGGTCCCCCTA[T/C]ATGCAGGATCTTGATTAGTCGTGTGAACAACTGAAATTTGAGCGCCACAA
```

### SNP list

If the flanking sequence is unknown, but the position on a reference is available, the option ```--snp_list``` can be
used and the FASTA file with the reference sequence must be provided with the option ```--reference```. This is to allow
the use of a different assembly or set of contigs used for the discovery of the SNPs that are different to the reference
given in the option ```--contigs```. The format contains the following positional columns:

* **scaffold** The sacffold where the SNP is.
* **reference allele** The base in the reference (may or may not be the same as in the reference file).
* **position** SNP position. The first base in the scaffold is base 1.
* **alternative allele** The base in the alternative allele.
* **target chromosome** for the specific primers. Must be in line with the chromosome selection criteria.

#### Example

```
IWGSC_CSS_1AL_scaff_110,C,519,A,2A
```

This file format can be used with ```snp_positions_to_polymarker.rb``` to produce the input for the option
```--marker_list```.

### Custom reference sequences.

By default, the contigs and pseudo-molecules
from [ensembl](ftp://ftp.ensemblgenomes.org/pub/release-25/plants/fasta/triticum_aestivum/dna/Triticum_aestivum.IWGSC2.25.dna.genome.fa.gz
) are used. However, it is possible to use a custom reference. To define the chromosome where each contig belongs the
argument ```arm_selection``` is used. The default uses ids like: ```IWGSC_CSS_1AL_scaff_110```, where the third field,
separated by underscores is used. A simple way to add custom references is to rename the fasta file to follow that
convention. Another way is to use the option ```--arm_selection arm_selection_first_two```, where only the first two
characters in each contig is used as identifier, useful when pseudo-molecules are named after the chromosomes (ie: ">1A"
in the fasta file).

If your contigs follow a different convention, in the file ```ChromosomeArm.rb``` it is possible to define new parsers,
by adding at the beginning, with the rest of the parsers a new lambda like:

```rb
@@arm_selection_functions[:embl] = lambda do |contig_name|
  arr = contig_name.split('_')
  ret = "U"
  ret = arr[2][0, 2] if arr.size >= 3
  ret = "3B" if arr.size == 2 and arr[0] == "v443"
  ret = arr[0][0, 2] if arr.size == 1
  return ret
end
```

The function should return a 2 character string, when the first is the chromosome number and the second the chromosome
group. The symbol in the hash is the name to be used in the argument ```--arm_selection```. If you want your parser to
be added to the distribution, feel free to fork and make a pull request.

##Using blast

To use blast instead of exonerate, use the following command:

```
./bin/polymarker.rb --contigs test/data/BS00068396_51_contigs.fa --marker_list test/data/BS00068396_51_for_polymarker.fa  --aligner blast  -a arm_selection_first_two
```
