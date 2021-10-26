# Домашнее задание №1
* Используемые команды

```
mkdir hw_1
cd hw_1/

ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s1016 oil_R1.fastq 5000000 > subpe1.fq
seqtk sample -s1016 oil_R2.fastq 5000000 > subpe2.fq
seqtk sample -s1016 oilMP_S4_L001_R1_001.fastq 1500000 > submp1.fq
seqtk sample -s1016 oilMP_S4_L001_R2_001.fastq 1500000 > submp2.fq

mkdir fastqc
ls *.fq | xargs -P 1 -tI{} fastqc -o fastqc {}

platanus_trim subpe1.fq subpe2.fq
platanus_internal_trim submp1.fq submp2.fq

rm submp1.fq
rm submp2.fq
rm subpe1.fq
rm subpe2.fq

mkdir trimmed_fastq
mv *trimmed trimmed_fastq/
cd trimmed_fastq/
mkdir trimmed_fastqc

ls *trimmed | xargs -P 1 -tI{} fastqc -o trimmed_fastqc {}

mkdir trimmed_multiqc
multiqc -o trimmed_multiqc trimmed_fastqc
platanus assemble -t 1 -f subpe1.fq.trimmed subpe2.fq.trimmed 2> assemble.log
platanus scaffold -t 1 -c out_contig.fa -IP1 subpe1.fq.trimmed subpe2.fq.trimmed -OP2 submp1.fq.int_trimmed submp2.fq.int_trimmed2> scaffold.log
platanus gap_close -t 1 -c out_scaffold.fa -IP1 subpe1.fq.trimmed subpe2.fq.trimmed -OP2 submp1.fq.int_trimmed submp2.fq.int_trimmed 2> gapclose.log

more out_gapClosed.fa
echo scaffold1_cov231 > name
seqtk subseq out_gapClosed.fa name > longest.fa
```
