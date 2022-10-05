# hse22_hw1

Create symlinks: $(ln -s /usr/share/data-minor-bioinf/assembly/* . )

Create paired-end sample files: $(ls oil_R* | xargs -I {} -P 10 sh -c "seqtk sample -s 701 {} 5000000 > sub-{}")

Create mate-pairs sample files: $(ls oilMP_S4_L001_R* | xargs -I {} -P 10 sh -c "seqtk sample -s 701 {} 1500000 > sub-{}")

Create fasqc reports: $(fastqc sub-*)

Create multiqc reports: $(multiqc .)

Move report files to another subdirectory $(mkdir to_scp && mv *.zip to_scp/ mv multiqc_data/ to_scp/ && mv *.html to_scp/)

Trim paired-end: $(platanus trim sub-oil_R*)

Trim mate-pairs: $(platanus_internal_trim sub-oilMP_S4_L001_R*)

Move trimmed files to another dir: $(mkdir trim && mv *trimmed trim/)

Change workdir: $(cd trim/)

Create fasqc reports: $(fastqc *)

Create multiqc reports: $(multiqc .)

Move needed files to another dir and switch there: $( cd .. && mkdir contigs && mv trim/*trimmed contigs/ && cd contigs)

Assemble contigs from paired-end trimmed: $(platanus assemble –f sub-oil*.trimmed )

Create scaffolds from contigs and trimmed: $(platanus scaffold -c out_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed)

Close gaps: $(platanus gap_close -c out_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed)

Scp from home 

Remove everything: $(cd ~ && rm -fr .)

```
ln -s /usr/share/data-minor-bioinf/assembly/* . 
ls oil_R* | xargs -I {} -P 10 sh -c "seqtk sample -s 701 {} 5000000 > sub-{}"
ls oilMP_S4_L001_R* | xargs -I {} -P 10 sh -c "seqtk sample -s 701 {} 1500000 > sub-{}"
fastqc sub-*
multiqc .
mkdir to_scp && mv *.zip to_scp/ mv multiqc_data/ to_scp/ && mv *.html to_scp/
platanus_trim sub-oil_R*
platanus_internal_trim sub-oilMP_S4_L001_R*
mkdir trim && mv *trimmed trim/
cd trim/
fastqc *
multiqc .
cd .. && mkdir contigs && mv trim/*trimmed contigs/ && cd contigs
platanus assemble –f sub-oil*.trimmed
platanus scaffold -c out_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed
platanus gap_close -c out_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed
```

