# Домашнее задание №1
## Часть 1
### Используемые команды
***Рассчётная часть произведена на Python. Результаты лежат в .ipynb файле в папке src***
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

more out_scaffold.fa
echo scaffold1_len3832127_cov231 > name
seqtk subseq out_scaffold.fa name > longest.fa

rm name

more out_gapClosed.fa
echo scaffold1_cov231 > name
seqtk subseq out_gapClosed.fa name > longest_closed.fa
```
### Скриншоты
**Важно: исходные .html файлы с полной статистикой находятся в папке data**
* Для исходных чтений
![Screenshot from 2021-10-26 12-48-47](https://user-images.githubusercontent.com/60858323/138952006-ec0f7e8e-8e49-4558-8c31-f6548155795c.png)
![Screenshot from 2021-10-26 12-49-33](https://user-images.githubusercontent.com/60858323/138952014-fd6a39e7-c3f9-496c-81e2-beb023f32a9d.png)
![Screenshot from 2021-10-26 12-49-58](https://user-images.githubusercontent.com/60858323/138952015-2c97937f-572f-4bc3-a5a0-99f67a2de463.png)
![Screenshot from 2021-10-26 12-50-13](https://user-images.githubusercontent.com/60858323/138952016-def2c51e-483b-45a6-993b-769bbb279d39.png)
![Screenshot from 2021-10-26 12-50-33](https://user-images.githubusercontent.com/60858323/138952018-93d4af7e-e5f1-4508-afd0-c2f6eda48c62.png)
![Screenshot from 2021-10-26 12-50-51](https://user-images.githubusercontent.com/60858323/138952020-165e7a61-1839-456e-a09f-32608aee2b68.png)
![Screenshot from 2021-10-26 12-51-05](https://user-images.githubusercontent.com/60858323/138952021-cd771bd1-0792-427d-a699-070252435fab.png)
![Screenshot from 2021-10-26 13-07-43](https://user-images.githubusercontent.com/60858323/138953393-d0da755c-8c72-44e9-a678-b6ac4c047354.png)
![Screenshot from 2021-10-26 12-51-34](https://user-images.githubusercontent.com/60858323/138952023-cfe93bb8-3c0c-49ab-8466-7b3d96f0e875.png)
![Screenshot from 2021-10-26 12-51-48](https://user-images.githubusercontent.com/60858323/138952024-f75d10ad-6bfa-4cfb-893f-606367b7421d.png)
![Screenshot from 2021-10-26 12-52-02](https://user-images.githubusercontent.com/60858323/138952025-3b1a6824-b2b4-4477-ae71-de940143bb19.png)
* Для подрезанных чтений
![Screenshot from 2021-10-26 13-01-51](https://user-images.githubusercontent.com/60858323/138953191-98d23f89-8397-4277-8df8-de17e4c1595c.png)
![Screenshot from 2021-10-26 13-02-19](https://user-images.githubusercontent.com/60858323/138953195-be686bec-13e7-4055-8464-0f8982d5d7bb.png)
![Screenshot from 2021-10-26 13-02-51](https://user-images.githubusercontent.com/60858323/138953197-27774255-d5b0-4fd9-9b37-e4dc9ae20ac4.png)
![Screenshot from 2021-10-26 13-03-11](https://user-images.githubusercontent.com/60858323/138953175-5089e512-3e49-47ab-8341-b7494a720c07.png)
![Screenshot from 2021-10-26 13-03-29](https://user-images.githubusercontent.com/60858323/138953179-6635f129-ab0d-4bc2-b01a-36bc00ea0fa5.png)
![Screenshot from 2021-10-26 13-03-51](https://user-images.githubusercontent.com/60858323/138953181-cbbd9ce5-8615-4ebf-8ea8-0dfc143e7f0c.png)
![Screenshot from 2021-10-26 13-04-07](https://user-images.githubusercontent.com/60858323/138953184-6bac0bef-bba0-4edd-9e92-4eb26a97fde6.png)
![Screenshot from 2021-10-26 13-04-21](https://user-images.githubusercontent.com/60858323/138953185-f2dcec02-4b65-4801-aa19-1f48c907f469.png)
![Screenshot from 2021-10-26 13-04-46](https://user-images.githubusercontent.com/60858323/138953186-f988e467-6806-4e77-a25e-1f7b4757d8aa.png)
![Screenshot from 2021-10-26 13-05-05](https://user-images.githubusercontent.com/60858323/138953188-46f9ae3d-c19e-412b-a277-51d2bbbfa601.png)
![Screenshot from 2021-10-26 13-05-19](https://user-images.githubusercontent.com/60858323/138953190-cd1d41f8-1874-4e67-acdc-c003f480028d.png)

## Часть 2
### Используемые команды
```
cd ~/hw_1
mkdir part80
mkdir part60
mkdir part40
mkdir part20

cd part80

ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s1016 oil_R1.fastq 4000000 > subpe1_80.fq
seqtk sample -s1016 oil_R2.fastq 4000000 > subpe2_80.fq
seqtk sample -s1016 oilMP_S4_L001_R1_001.fastq 1200000 > submp1_80.fq
seqtk sample -s1016 oilMP_S4_L001_R2_001.fastq 1200000 > submp2_80.fq

platanus_trim subpe1_80.fq subpe2_80.fq
platanus_internal_trim submp1_80.fq submp2_80.fq

platanus assemble -t 1 -f subpe1_80.fq.trimmed subpe2_80.fq.trimmed 2> assemble.log
platanus scaffold -t 1 -c out_contig.fa -IP1 subpe1_80.fq.trimmed subpe2_80.fq.trimmed -OP2 submp1_80.fq.int_trimmed submp2_80.fq.int_trimmed 2> scaffold.log
platanus gap_close -t 1 -c out_scaffold.fa -IP1 subpe1_80.fq.trimmed subpe2_80.fq.trimmed -OP2 submp1_80.fq.int_trimmed submp2_80.fq.int_trimmed 2> gapclose.log

more out_scaffold.fa
echo scaffold1_len3836315_cov184 > name
seqtk subseq out_scaffold.fa name > longest80.fa

rm name

more out_gapClosed.fa
echo scaffold1_cov184 > name
seqtk subseq out_gapClosed.fa name > longest_closed80.fa

cd ../part60

ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s1016 oil_R1.fastq 3000000 > subpe1_60.fq
seqtk sample -s1016 oil_R2.fastq 3000000 > subpe2_60.fq
seqtk sample -s1016 oilMP_S4_L001_R1_001.fastq 900000 > submp1_60.fq
seqtk sample -s1016 oilMP_S4_L001_R2_001.fastq 900000 > submp2_60.fq

platanus_trim subpe1_60.fq subpe2_60.fq
platanus_internal_trim submp1_60.fq submp2_60.fq

platanus assemble -t 1 -f subpe1_60.fq.trimmed subpe2_60.fq.trimmed 2> assemble.log
platanus scaffold -t 1 -c out_contig.fa -IP1 subpe1_60.fq.trimmed subpe2_60.fq.trimmed -OP2 submp1_60.fq.int_trimmed submp2_60.fq.int_trimmed 2> scaffold.log
platanus gap_close -t 1 -c out_scaffold.fa -IP1 subpe1_60.fq.trimmed subpe2_60.fq.trimmed -OP2 submp1_60.fq.int_trimmed submp2_60.fq.int_trimmed 2> gapclose.log


more out_scaffold.fa
echo scaffold2_len3831102_cov140 > name
seqtk subseq out_scaffold.fa name > longest60.fa

rm name

more out_gapClosed.fa
echo scaffold1_cov71 > name
seqtk subseq out_gapClosed.fa name > longest_closed60.fa

cd ../part40

ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s1016 oil_R1.fastq 2000000 > subpe1_40.fq
seqtk sample -s1016 oil_R2.fastq 2000000 > subpe2_40.fq
seqtk sample -s1016 oilMP_S4_L001_R1_001.fastq 600000 > submp1_40.fq
seqtk sample -s1016 oilMP_S4_L001_R2_001.fastq 600000 > submp2_40.fq

platanus_trim subpe1_40.fq subpe2_40.fq
platanus_internal_trim submp1_40.fq submp2_40.fq

platanus assemble -t 1 -f subpe1_40.fq.trimmed subpe2_40.fq.trimmed 2> assemble.log
platanus scaffold -t 1 -c out_contig.fa -IP1 subpe1_40.fq.trimmed subpe2_40.fq.trimmed -OP2 submp1_40.fq.int_trimmed submp2_40.fq.int_trimmed 2> scaffold.log
platanus gap_close -t 1 -c out_scaffold.fa -IP1 subpe1_40.fq.trimmed subpe2_40.fq.trimmed -OP2 submp1_40.fq.int_trimmed submp2_40.fq.int_trimmed 2> gapclose.log

more out_scaffold.fa
echo scaffold1_len3822028_cov91 > name
seqtk subseq out_scaffold.fa name > longest40.fa

rm name

more out_gapClosed.fa
echo scaffold1_cov91 > name
seqtk subseq out_gapClosed.fa name > longest_closed40.fa
```

![stats](https://user-images.githubusercontent.com/60858323/139141108-d6c2bd05-42a2-4dac-a9d7-e55a4a6c19e6.jpg)
