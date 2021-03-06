---
title: De-novo transcriptome analysis
subtitle: De-novo transcriptome analysis

# Summary for listings and search engines
summary: 

# Link this post with a project
projects: [Bioinformatics]

# Date published
date: "2021-4-3T00:00:00Z"

# Date updated
lastmod: "2020-2-3T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: ''
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin

tags:
- 转录组
- Trinity
- 比对
- 定量

categories:
- 无参转录组
- 教程
---

## 无参转录组分析实操

### Author：小虫子

### 参考：StatQuest [High Throughput Sequencing]

#### Background

(https://www.youtube.com/playlist?list=PLblh5JKOoLUJo2Q6xK4tZElbIvAACEykp)
![mark](http://cdn.liguocheng.top/blog/20200409/VLerTjISO6cV.png?imageslim)
![mark](http://cdn.liguocheng.top/blog/20200409/GavgM6BTyVTG.png?imageslim)
![mark](http://cdn.liguocheng.top/blog/20200409/Yvvl0O7y0KkO.png?imageslim)

#### library construction

![mark](http://cdn.liguocheng.top/blog/20200409/oIoecekwQSBD.png?imageslim)

#### Sequencing

![mark](http://cdn.liguocheng.top/blog/20200409/WJuQvPrb5cB1.png?imageslim)

#### Mapping

![mark](http://cdn.liguocheng.top/blog/20200409/WQy4R2fLLXj4.png?imageslim)

#### Reads_count

![mark](http://cdn.liguocheng.top/blog/20200409/JS4uvmiPcAIu.png?imageslim)

#### Normalization

##### FPKM

(Reads Per Kilobase of exon model per Million mapped reads)
RPKM= total exon reads/ (mapped reads (Millions) * exon length(KB)
 total exon reads：某个样本mapping到特定基因的外显子上的所有的reads。
 mapped reads (Millions) :某个样本的所有reads总和。
 exon length(KB)：某个基因的长度（外显子的长度的总和，以KB为单位）。
![mark](http://cdn.liguocheng.top/blog/20200409/6NR271k74ByP.png?imageslim)

1. read counts 除以基因长度
    ![mark](http://cdn.liguocheng.top/blog/20200409/6Dc3wBoaTGdj.png?imageslim)
    2.将每个样品的 read counts 合并，添加一行 Total Reads
    ![mark](http://cdn.liguocheng.top/blog/20200409/PzDyfJTd9AOW.png?imageslim)

2. 再除以 Total，得到以下表格
    ![mark](http://cdn.liguocheng.top/blog/20200409/Wns1qwP6WRhs.png?imageslim)

  ##### TPM

  (Transcripts Per Kilobase Million)
  (推荐软件，RSEM/Stringtie) 的计算公式：
  TPMi=(Ni/Li)*1000000/sum(Ni/Li+……..+ Nm/Lm)
  Ni：mapping到基因i上的read数； Li：基因i的外显子长度的总和

3. 将每个样品的数值求和，添加一行 Total
    ![mark](http://cdn.liguocheng.top/blog/20200409/Pq7DvMgC2yrO.png?imageslim)

4. 再除以 Total，得到以下表格
    ![mark](http://cdn.liguocheng.top/blog/20200409/IYMy7IKQpf7o.png?imageslim)

  #### Visualization

![mark](http://cdn.liguocheng.top/blog/20200409/LJmvDOXSB69U.png?imageslim)

  #### Differential expression genes

  ![mark](http://cdn.liguocheng.top/blog/20200409/spbTn4xiwanU.png?imageslim)

#### 1. 数据质控与过滤

##### FASTQ格式说明：

  @HWUSI-EAS100R:6:73:941:1973#0/1
  GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTT
  +HWUSI-EAS100R:6:73:941:1973#0/1
  !''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC6

- 其中第一行以@开头，后面是reads的ID以及其他信息，例如上例中 HWUSI-EAS100R代表Illumina设备名称，6代表flowcell中的第六个lane，73代表第六个lane中的第73个tile，941:1973代表该read在该tile中的x：y坐标信息；#0，若为多样本的混合作为输入样本，则该标志代表样本的编号，用来区分个样本中的reads；/1代表paired end中的前一个read。
- 第二行为read的序列。
- 第三行以“+”开头，跟随着该read的名称（一般于@后面的内容相同），但有时可以省略，但“+”一定不能省。
- 最初sanger中心用Phred quality score来衡量该read中每个碱基的质量，既-10lgP ，其中P代表该碱基被测序错误的概率，如果该碱基测序出错的概率为0.001，则Q应该为30，那么30+33=63，那么63对应的ASCii码为“？”，则在第四行中该碱基对应的质量代表值即为"?"。
PHRED33: Convert quality scores to Phred-33
PHRED64: Convert quality scores to Phred-64

##### Trimmomatic 

Example: 
java -jar trimmomatic-0.35.jar PE -phred33 input_forward.fq.gz input_reverse.fq.gz output_forward_paired.fq.gz output_forward_unpaired.fq.gz output_reverse_paired.fq.gz output_reverse_unpaired.fq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36

- ILLUMINACLIP: Cut adapter and other illumina-specific sequences from the read.
- SLIDINGWINDOW: Perform a sliding window trimming, cutting once the average quality within the window falls below a threshold.
- LEADING: Cut bases off the start of a read, if below a threshold quality
- TRAILING: Cut bases off the end of a read, if below a threshold quality
- MINLEN: Drop the read if it is below a specified length

>This will perform the following:
>Remove adapters (ILLUMINACLIP:TruSeq3-PE.fa:2:30:10)
>Remove leading low quality or N bases (below quality 3) (LEADING:3)
>Remove trailing low quality or N bases (below quality 3) (TRAILING:3)
>Scan the read with a 4-base wide sliding window, cutting when the average quality per base drops below 15 (SLIDINGWINDOW:4:15)
>Drop reads below the 36 bases long (MINLEN:36)
```
cat filter.sh
trimmomatic PE -phred33 \
/Storage/data003/ligc/jiangnanRAWdata/Msep_F_Ant_1.fq.gz \
/Storage/data003/ligc/jiangnanRAWdata/Msep_F_Ant_2.fq.gz \
./Msep_F_Ant_1_paired.fq.gz ./Msep_F_Ant_1_unpaired.fq.gz \
./Msep_F_Ant_2_paired.fq.gz ./Msep_F_Ant_2_unpaired.fq.gz \
ILLUMINACLIP:/Storage/data003/ligc/anaconda3/share/trimmomatic/adapters/TruSeq3-PE-2.fa:2:30:10 LEADING:5 TRAILING:5 SLIDINGWINDOW:4:5 MINLEN:25
```
#### 2. Trinity assembly

##### Workflow

![mark](http://cdn.liguocheng.top/blog/20200409/2SmB0IEVnTOB.png?imageslim)

##### 原理：

**Inchworm module**：将reads打断成kmer，通过对kmer的延伸构建出contig序列。
**Chrysalis module**：根据contig之间(k-1)mer的重叠关系，构建出De Bruijn graph，理想情况下一张图代表一个基因，不同的路径代表不同的剪切形式。
**Butterfly module**：根据reads对各个路径的支持选择最优路径，打印输出。
![mark](http://cdn.liguocheng.top/blog/20200409/g5t713zvLSDB.png?imageslim)
![mark](http://cdn.liguocheng.top/blog/20200409/LLutSmx0TI9S.png?imageslim)
![mark](http://cdn.liguocheng.top/blog/20200409/HKmVery80svd.png?imageslim)



![mark](http://cdn.liguocheng.top/blog/20200409/IMevvKiSlDux.png?imageslim)

![mark](http://cdn.liguocheng.top/blog/20200409/QedbSkMJduuG.png?imageslim)





**Inchworm** assembles the RNA-seq data into the unique sequences of transcripts, often generating full-length transcripts for a dominant isoform, but then reports just the unique portions of alternatively spliced transcripts.

**Chrysalis** clusters the Inchworm contigs into clusters and constructs complete de Bruijn graphs for each cluster. Each cluster represents the full transcriptonal complexity for a given gene (or sets of genes that share sequences in common). Chrysalis then partitions the full read set among these disjoint graphs.

**Butterfly** then processes the individual graphs in parallel, tracing the paths that reads and pairs of reads take within the graph, ultimately reporting full-length transcripts for alternatively spliced isoforms, and teasing apart transcripts that corresponds to paralogous genes.
```
nohup /Storage/data003/ligc/software/trinityrnaseq-Trinity-v2.8.4/Trinity \
 --seqType fq \
 --max_memory 60 \
 --samples_file /Storage/data003/ligc/jiangnanRAWdata/sample.txt \
 --CPU 30 \
 > trinity.log 2> trinity_err.log &
```
#### 3. Transcripts quality assessment

##### 3.1 Trinity_Stats

```
TrinityStats.pl ../MsepTrinity.fa > Assembly_Stats.txt
```
![mark](http://cdn.liguocheng.top/blog/20200409/wfGlFmakYYbm.png?imageslim)
> 拼接的转录本数量一般在20万条以内都可以，转录本数量远多于基因数量，如果转录本数量过多可以考虑使用corset聚类。

##### 3.2 BUSCO(Benchmarking Universal Single-Copy Orthologs)

BUSCO原理:在近缘物种中，总有一些基因是保守的。那么，我们就来看这些保守的基因拼接的完整性和准确性如何。BUSCO 软件根据OrthoDB 数据库，构建了几个大的进化分支的单拷贝基因集。将转录本拼接结果与该基因集进行比较，根据比对上的比例、完整性，来评价拼接结果的准确性和完整性。

```
#下载数据库(目前本版本可以自动下载数据库)
wget http://busco.ezlab.org/v2/datasets/insecta_odb9.tar.gz
#目前数据库更新到v10了
(base) [ligc@cluster busco]$ cat Run_busco.sh
#获取每个基因最长转录本
perl /pub/anaconda3/opt/trinity-2.8.4/util/misc/get_longest_isoform_seq_per_trinity_gene.pl ../../MsepTrinity.fa >longest_isoform.fasta

#运行busco
busco -i longest_isoform.fasta \
        -l insecta_odb10 \
        -o busco \
        -m tran \
        --cpu 10
```
![short summary](http://cdn.liguocheng.top/blog/20200409/sXsOAuiQM93b.png?imageslim)
可以看到有85%的转录本序列都可以比对到完整的BUSCO.

#### 4. Annotation

下载数据库文件

```bash
(base) [ligc@cluster Trinotate]$ cat Build_Trinotate_db.sh
#下载database: uniprot ;eggnog ;GO ;Pfam ;pfam2go
perl /Storage/data003/ligc/software/Trinotate-Trinotate-v3.1.1/admin/Build_Trinotate_Boilerplate_SQLite_db.pl Trinotate20190422
##构建blast database;diamond的速度更快
# makeblastdb -in uniprot_sprot.pep -dbtype prot
diamond makedb --db new_uniprot_sprot.pep --in new_uniprot_sprot.pep
##构建HMMER database
gunzip Pfam-A.hmm.gz
hmmpress Pfam-A.hmm
##去除空行
awk 'BEGIN {RS = ">" ; FS = "\n" ; ORS = ""} $2 {print ">"$0}' uniprot_sprot.pep > new_uniprot_sprot.pep
```
![mark](http://cdn.liguocheng.top/blog/20200409/F8DDaEVrnHXN.png?imageslim)

```
[ligc@cluster step_1_run_transdecoder]$ cat 1_run_transdecoder.sh
#!/bin/bash
transcripts=/Storage/data003/ligc/raw_data/MsepTrinity.fa
uniprot_sprot_db=/Storage/data003/ligc/database/Trinotate/new_uniprot_sprot.pep
Pfam=/Storage/data003/ligc/database/Trinotate/Pfam-A.hmm
#1.初步筛选ORF
TransDecoder.LongOrfs -t $transcripts
##生成longest_orfs.pep，与已知蛋白数据库进行比对，提高ORF预测的准确性
#2.与已知蛋白数据库比对（可选）
diamond blastp -q /Storage/data003/ligc/Annotation/step_1_run_transdecoder/MsepTrinity.fa.transdecoder_dir/longest_orfs.pep \
             --db $uniprot_sprot_db  \
             --max-target-seqs 1 \
             --outfmt 6 \
             --evalue 1e-10 \
             --threads 20 \
             > MsepTrinity.fa.transdecoder_dir/longest_orfs.pep.blastp.outfmt6
##生成longest_orfs.pep.blastp.outfmt6
hmmscan --cpu 35 \
        --domtblout /Storage/data003/ligc/Annotation/step_1_run_transdecoder/MsepTrinity.fa.transdecoder_dir/longest_orfs.pep.pfam.domtblout \
        $Pfam \
/Storage/data003/ligc/Annotation/step_1_run_transdecoder/MsepTrinity.fa.transdecoder_dir/longest_orfs.pep
##生成longest_orfs.pep.pfam.domtblout
#3.进一步筛选ORF(如果一个ORF在另一个ORF内部则保留长的ORF）
TransDecoder.Predict -t $transcripts \
            --retain_pfam_hits MsepTrinity.fa.transdecoder_dir/longest_orfs.pep.pfam.domtblout \
            --retain_blastp_hits MsepTrinity.fa.transdecoder_dir/longest_orfs.pep.blastp.outfmt6
##生成MsepTrinity.fa.transdecoder.pep；MsepTrinity.fa.transdecoder.bed；MsepTrinity.fa.transdecoder.cds；MsepTrinity.fa.transdecoder.gff3
```
![mark](http://cdn.liguocheng.top/blog/20200409/x3R1W53FR5pU.png?imageslim)

> hmmer 软件包中的hmmscan命令用于功能注释，而hmmsearch则用于抗性基因的鉴定和转录因子的发现。

![mark](http://cdn.liguocheng.top/blog/20200409/Od23b21rRoyG.png?imageslim)

```bash
[ligc@cluster step_2_run_blast_hmmer]$ cat step_2_run_blast_hmmer.sh
$transcripts=/Storage/data003/ligc/raw_data/MsepTrinity.fa
$proteins=/Storage/data003/ligc/Annotation/step_1_run_transdecoder/MsepTrinity.fa.transdecoder.pep
$uniprot_sprot_db=/Storage/data003/ligc/database/Trinotate/new_uniprot_sprot.pep
$Pfam=/Storage/data003/ligc/database/Trinotate/Pfam-A.hmm
#blastx -query $transcripts -db $uniprot_sprot_db -num_threads 8 -max_target_seqs 1 -outfmt 6 > blastx.outfmt6
diamond blastx --query $transcripts --db $uniprot_sprot_db --threads 35 --max-target-seqs 1 --outfmt 6 > blastx.outfmt6
#blastp -query $proteins -db $uniprot_sprot_db -num_threads 8 -max_target_seqs 1 -outfmt 6 > blastp.outfmt6
diamond blastp --query $proteins --db $uniprot_sprot_db --threads 35 --max-target-seqs 1 --outfmt 6 > blastp.outfmt6
hmmscan --cpu 35 --domtblout TrinotatePFAM.out $Pfam $proteins > pfam.log
```
blast/diamond tabular 格式说明：
![blast](http://cdn.liguocheng.top/blog/20200409/25eJaTxe1ADG.png?imageslim)
12列对应的含义依次是：
    Query id：           查询序列ID标识
    Subject id：         比对上的目标序列ID标识
    % identity：         序列比对的一致性百分比
    alignment length：符合比对的比对区域的长度
    mismatches：      比对区域的错配数
    gap openings：    比对区域的gap数目
    q. start：             比对区域在查询序列(Query id)上的起始位点
    q. end：              比对区域在查询序列(Query id)上的终止位点
    s. start：             比对区域在目标序列(Subject id)上的起始位点
    s. end：              比对区域在目标序列(Subject id)上的终止位点
    e-value：            比对结果的期望值
    bit score：          比对结果的bit score值

> **uniprot id可以mapping到GO,KEGG,eggNOG数据库**

![mark](http://cdn.liguocheng.top/blog/20200409/c24eiroNPQv2.png?imageslim)



![mark](http://cdn.liguocheng.top/blog/20200409/fiFlUrtYsDXM.png?imageslim)

```bash
[ligc@cluster step_3_run_trinotate]$ cat step3_run_trinotate.sh
#/bin/bash
TRINOTATE_HOME=/Storage/data003/ligc/software/Trinotate-Trinotate-v3.1.1
transcripts=/Storage/data003/ligc/raw_data/MsepTrinity.fa
proteins=/Storage/data003/ligc/Annotation/step_1_run_transdecoder/MsepTrinity.fa.transdecoder.pep
gene_trans_map=/Storage/data003/ligc/raw_data/MsepTrinity.fa.gene_trans_map
Trinotate_sqlite=/Storage/data003/ligc/database/Trinotate/Trinotate20190422.sqlite
blastp_outfmt6=/Storage/data003/ligc/Annotation/step_2_run_blast_hmmer/blastp.outfmt6
blastx_outfmt6=/Storage/data003/ligc/Annotation/step_2_run_blast_hmmer/blastx.outfmt6
pfam_out=/Storage/data003/ligc/Annotation/step_2_run_blast_hmmer/TrinotatePFAM.out

#copy Trinotate.sqlite
#cp $Trinotate_sqlite ./Trinotate.sqlite

#init
$TRINOTATE_HOME/Trinotate Trinotate.sqlite init --gene_trans_map $gene_trans_map --transcript_fasta $transcripts --transdecoder_pep $proteins

# load protein hits
$TRINOTATE_HOME/Trinotate Trinotate.sqlite LOAD_swissprot_blastp $blastp_outfmt6

# load transcript hits
$TRINOTATE_HOME/Trinotate Trinotate.sqlite LOAD_swissprot_blastx $blastx_outfmt6

# load pfam hits
$TRINOTATE_HOME/Trinotate Trinotate.sqlite LOAD_pfam $pfam_out

# report
$TRINOTATE_HOME/Trinotate Trinotate.sqlite report > trinotate_annotation_report.xls
```
#### 5. Expression quantification

##### 原理：

1.在不同背景下比较mRNA水平

同一物种，不同组织：研究基因在不同部分的表达情况
同一物种，同一组织：研究基因在不同处理下，不同条件下的表达变化
同一组织，不同物种：研究基因的进化关系
时间序列实验： 基因在不同时期的表达情况与发育的关系

**我们在进行转录组测序的时候，并不知道用于建库测序的 RNA 来自多少个细胞，总过有多少转录本。所以只能进行相对定量(scRNA-seq)。相对定量，描述的是该基因的转录本占样本中所有转录本的百分比。**
![mark](http://cdn.liguocheng.top/blog/20200409/QfKFacxjQiFU.png?imageslim)
![mark](http://cdn.liguocheng.top/blog/20200409/RbIgRDTTQjMV.png?imageslim)
TPM 中的![mark](http://cdn.liguocheng.top/blog/20200409/8GEo3mN2kbnt.png?imageslim)是对所有基因转录本数目的求和。FPKM 的公式中，![mark](http://cdn.liguocheng.top/blog/20200409/9i66VLtXAugo.png?imageslim)没有考虑基因长度的影响，相当于直接使用了总 Fragments 数目。
所以，TPM 更符合我们对相对表达量的定义，而FPKM 似乎没有明确的意义。

##### 脚本：

```bash
(base) [ligc@cluster quantification]$ cat quantify.sh
perl /Storage/data003/ligc/software/trinityrnaseq-Trinity-v2.8.4/util/align_and_estimate_abundance.pl --transcripts /Storage/data003/ligc/raw_data/MsepTrinity.fa --seqType fq --est_method RSEM --aln_method bowtie2 --prep_reference --trinity_mode --samples_file /Storage/data003/ligc/jiangnanRAWdata/sample.txt --thread_count 40

ls */RSEM.genes.results > genes.quant_files.txt
perl /Storage/data003/ligc/software/trinityrnaseq-Trinity-v2.8.4/util/abundance_estimates_to_matrix.pl --est_method RSEM --cross_sample_norm TMM --name_sample_by_basedir --gene_trans_map /Storage/data003/ligc/raw_data/MsepTrinity.fa.gene_trans_map --quant_files isoforms.quant_files.txt --out_prefix isoforms
```
align_and_estimate_abundance.pl 将原始reads比对到组装好的转录本上生成各个样本基因的相对表达量矩阵（TPM,以及FPKM值）。
abundance_estimates_to_matrix.pl 将不同样本的表达量合并为一个表达矩阵。
#####结果：
![mark](http://cdn.liguocheng.top/blog/20200409/0syhlMhDEpvz.png?imageslim)

```
less -S RSEM.genes.results
```
![mark](http://cdn.liguocheng.top/blog/20200409/U2D59RVcTLth.png?imageslim)

#### 6. Differential Expression Analysis

> 使用edgeR进行无生物学重复的差异表达分析

```bash
[ligc@cluster DE_analysis]$ cat run_DE_analysis.sh
#/bin/bash
TRINITY_HOME = /Storage/data003/ligc/software/trinityrnaseq-Trinity-v2.8.4
isoform_counts_matrix = /Storage/data003/ligc/quantification/genes.isoform.counts.matrix
samples_files = /Storage/data003/ligc/jiangnanRAWdata/sample.txt

perl /Storage/data003/ligc/software/trinityrnaseq-Trinity-v2.8.4/Analysis/DifferentialExpression/run_DE_analysis.pl \
    --matrix /Storage/data003/ligc/quantification/isoforms.isoform.counts.matrix \
    --method edgeR \
    --dispersion 0.1
    --samples_file /Storage/data003/ligc/jiangnanRAWdata/sample.txt
```
#####结果分析：
![mark](http://cdn.liguocheng.top/blog/20200409/uEdeTMq9Gpfv.png?imageslim)
**logFC : log(fold change) 表示两个样本间表达量的倍数的对数。**
**logCPM : log(counts per millions) 可以理解为未经过基因长度标准化的表达量的对数。**
**PValue：**
**FDR:**
**DEG_list : FDR < 0.05 && | logFC | >1**

#### 7.Normalization

一：样品间（组间的数据）的表达标准化
(处理样品中，有一个基因的表达量异常的高，大量的reads被占，导致其他gene的相对表达量下降。)TPM的标准化是在各自的样品内部的差异基因相对表达量。对于组间来说，也就是处理组和对照组来说就需要进一步的标准化。目的就是能够得到样品间的绝对表达量是否有差异。
标准化方法：
内参基因，看家基因。某一基因在样本中的表达肯定是一样的。(有限制，依赖基因的功能注释，数目少准确不高。
![mark](http://cdn.liguocheng.top/blog/20200409/sGJRkgrIwjlT.png?imageslim)
解决方法2：假设大多数基因是没有差异表达的。用统计学方法找到标准化因子。
DESeq2(estimateSizeFactors/sizeFactors) edgeR（TMM,calcNormFactors）
*可用Trinity软件包里的run_DE_analysis.pl*
二：假设检验进行差异表达基因的鉴定
1.建库测序是一个随机抽样的过程。纵使是两次测序，即抽样，同一基因的reads_count也会有差异。
当观察到的基因在两组reads_count 中不一样的时候，可能有两种原因：可能是表达丰度上的差异，也可能是随机抽样过程中的波动。假设是随机波动导致的，然后计算在随机波动的前提下，出现当前事件的概率，如果概率低。
2.假设检验：需要根据实际情况（总体均值、方差是否已知，单样本还是多样本，重复个数的多少/大样本量还是小样本量）
统计方法：z检验；t检验；卡方检验
3.差异表达基因通常用t检验（组间差异除以/组内差异，组间差异更大，而组内差异小则更可能有差异【此即是需要测更多生物学重复，能更准确的估计组内差异】）
4.t值的双侧检验和单侧检验。
单侧检验：G1在处理组中表达 大于 对照组。右侧面积->则单侧检验，即up-regulate。
双侧检验：G1处理组与对照组有明显差异。则双侧检验
5.一类错误(即为假阳性，与P-value对应，0.05 or 0.01)，二类错误(指漏诊率,假阴性)
降低一类错误：对于一条gene可能有5%误诊，而在差异基因鉴定时都是大规模的，如果一万条，50条误诊则太高了。p-value 进一步的矫正： FDR，q-value
降低二类错误：G3增加生物学重复，G2增加测序量来提高抽样次数。
![mark](http://cdn.liguocheng.top/blog/20200409/tyKz733fdCVu.png?imageslim)

#### 8.GO&KEGG pathway Enrichment Analysis

**基因富集分析**是在一组基因中找到具有一定基因功能特征和生物过程的基因集，在研究差异表达基因、筛选基因的后续分析中经常使用。
**算法**
大体上富集分析有四类算法：ORA、FCS、PT、NT
![algorithm](http://cdn.liguocheng.top/blog/20200409/W5G1GeLHewkd.png?imageslim)
**【最常用】ORA(Over Representation Analysis)：过表达分析**
**GO富集分析：**
**推荐使用clusterProfiler软件包（R包）**

- Gene Ontology: 基因本体论，描述基因的层级关系【基于ORA算法】可以算得上是高通量数据分析的标配，转录组、甲基化、ChIP-seq、重测序等，都会用到对一个或多个集合的基因进行功能富集分析，来找这个基因集的功能偏好性。
- 它将基因分门别类放入细胞组分CC、分子功能MF和生物过程BP三个功能类别中（分别对应基因产物在哪里发挥功能、发挥什么样的功能、怎样发挥功能）。
**KEGG富集分析：**
Kyoto Encyclopedia of Genes and Genomes: 系统分析基因产物和化合物在细胞中的代谢途径以及这些基因产物的功能的数据库【基于ORA算法】
- 整合了基因组、化学分子和生化系统等方面的数据,包括代谢通路（KEGG PATHWAY）、药物（KEGG DRUG）、疾病（KEGG DISEASE）、功能模型（KEGG MODULE）、基因序列（KEGG GENES）及基因组（KEGG GENOME）等等。
- KEGG 有一套完整KO注释的系统，可完成新测序物种的基因组或转录组的功能注释（KO是蛋白质或酶的一个分类体系，将同一条通路上功能相似、序列相似的蛋白质归为一类）。因此它可以将基因一个个归置到代谢网络指定位置上。

  #### 参考文章：

  1.[https://www.jianshu.com/p/042b888d5520](https://www.jianshu.com/p/042b888d5520)
  2.[https://www.jianshu.com/p/d09e624efcab](https://www.jianshu.com/p/d09e624efcab)
  3.[https://www.jianshu.com/p/1505fa220ce4](https://www.jianshu.com/p/1505fa220ce4)
  4.[https://www.jianshu.com/nb/14291282](https://www.jianshu.com/nb/14291282)
  5.[http://tiramisutes.github.io/2019/01/21/Trinity.html](http://tiramisutes.github.io/2019/01/21/Trinity.html)
  6.[https://www.jianshu.com/p/042b888d5520](https://www.jianshu.com/p/042b888d5520)
  7.[http://wap.sciencenet.cn/blog-1113671-1038659.html?mobile=1](http://wap.sciencenet.cn/blog-1113671-1038659.html?mobile=1)

  ## License

Copyright 2021 [Guo-Cheng Li](https://moths.com). Released under the [MIT](https://github.com/gcushen/hugo-academic/blob/master/LICENSE.md) license.