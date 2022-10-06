# hse22_hw1

1. ссылка на каждый из файлов

`ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq`

`ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq`

`ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq`

`ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq`

2. установка сидов и выбор сэмплов

`seqtk sample -s1704 oil_R1.fastq 5000000 > sub1.fastq`

`seqtk sample -s1704 oil_R2.fastq 5000000 > sub2.fastq`

`seqtk sample -s1704 oilMP_S4_L001_R1_001.fastq 1500000 > matepairs.fastq`

`seqtk sample -s1704 oilMP_S4_L001_R2_001.fastq 1500000 > matepairs2.fastq`

3. оценка качества 

`mkdir fastqc`

`ls sub* matepairs* | xargs -tI{} fastqc -o fastqc {}`

`mkdir multiqc`

`multiqc -o multiqc fastqc`

4. подрез чтений
 
`platanus_trim sub*`

`platanus_internal_trim matepair*`

5. удаление изначальных файлов

`rm sub* matep*`

6. оценка качества обрезанных чтений

`mkdir fastqc_trimmed`

`ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}`

7. сбор контиг

`time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log`

8. сбор скаффолдов

`time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> scaffold.log`

9. уменьшение числа промежутков

`time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> gap_close.log`

10. удаление подрезанных чтений

`rm sub*.trimmed matep*.int_trimmed`

для исходных данных
![dirty_data_1](https://user-images.githubusercontent.com/104971016/194349926-301dd2c2-ddea-4af8-9ecb-93e689b9e03b.png)
![dirty_data_2](https://user-images.githubusercontent.com/104971016/194350129-91cf5d82-48c6-44ca-8618-e4221d277267.png)
![dirty_data_3](https://user-images.githubusercontent.com/104971016/194350165-35cc2692-7959-4375-a4d1-585cb846e91d.png)
![dirty_data_4](https://user-images.githubusercontent.com/104971016/194350208-65c72263-1aa9-4769-90e3-01c7c8ef13d2.png)

для подрезанных данных
![clear_data_1](https://user-images.githubusercontent.com/104971016/194350917-aa77933a-81c2-4fb0-ae2b-dbd685ca2da2.png)
![clear_data_2](https://user-images.githubusercontent.com/104971016/194350991-e22ec7d4-d3e3-4796-8e20-3016c82293f9.png)
![clear_data_3](https://user-images.githubusercontent.com/104971016/194351049-9984b903-f687-453f-a545-6819673bfdf1.png)
![clear_data_4](https://user-images.githubusercontent.com/104971016/194351164-711cf363-582c-4703-8f3d-5ff132eded9e.png)

Общее количество контигов: 610

Общая длина контигов: 3924022

Длина самого длинного контига: 179307

N50: 47993

Общее количество скаффолдов: 69

Общая длина скаффолдов: 3873035

длина самого длинного скаффолда: 3831426

N50: 3831426

контиги: https://colab.research.google.com/drive/1atCTOuOQlgZ4-4gwlrkuKIpkpNdW5Qo7?usp=sharing#scrollTo=qpuWE4HXXkns

скаффолды: https://colab.research.google.com/drive/1XY4wsrEVf1W4Q_X-EUPIhwKO3XlfryUV?usp=sharing
