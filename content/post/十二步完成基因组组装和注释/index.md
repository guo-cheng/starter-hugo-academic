---
title: Twelve quick steps for genome assembly and annotation in the classroom
subtitle: 

# Summary for listings and search engines
summary: 

# Link this post with a project
projects: [Bioinformatics]

# Date published
date: "2021-5-3T00:00:00Z"

# Date updated
lastmod: "2020-6-3T00:00:00Z"

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
- genome assembly
- genome annotation

categories:
- Bioinformatics
- 教程
---

# Twelve quick steps for genome assembly and annotation in the classroom



![PLOS COMPUTATIONAL BIOLOGY](https://cdn.liguocheng.top//uPic/RWb24w.jpg)

​		真核基因组测序和从头组装，曾经是资金雄厚的国际财团的专属领域，现在已经变得越来越负担得起，因此适合各个研究小组的预算。第三代长读DNA测序技术越来越多地被使用，提供了大量的基因组工具箱，这些工具箱曾经是为少数精选的模式生物保留的。为许多水生物种生成高质量的基因组组装和注释仍然是一个巨大的挑战，因为它们的基因组大小、复杂性和高染色体数量。的确，为一个新的基因组项目选择最合适的测序和软件平台以及注释管道可能是令人望而生畏的，因为工具通常只在有限的环境下工作。在基因组学中，生成高质量的基因组组装/注释已经成为更好地理解任何物种生物学不可缺少的工具。在这里，我们陈述了12个步骤来帮助研究人员开始基因组计划，提出广泛适用(任何物种)、随着时间的推移可持续的指导方针，从头到尾涵盖基因组组装和注释计划的所有方面。我们回顾了一些常用的方法，包括提取高质量DNA的实用方法，以及最佳测序平台和文库准备的选择。此外，我们讨论了潜在的生物信息学管道的范围，包括结构和功能注释(例如，转座元件和重复序列)。本文还介绍了如何为基因组计划建立一个广泛的社区，数据管理的重要性，以及如何通过将数据和结果提交到公共存储库并与研究社区共享来使数据和结果可查找、可访问、可互操作和可重用(公平)。

### Step 1: Build a wide community for the project if possible

所有的基因组计划都有一个共同但不变的目标: 为广泛的基因组学应用而测序整个目标基因组。

### Step 2: Gather information about the target genome

- the genome size, levels of ploidy and heterozygosity, GC content, and complexity

#### **How big is the genome?**

publicly available databases: animals (http://www.genomesize.com), and plants (http://data.kew.org/cvalues).

基因组大小的评估：

the two widely used flow cytometry and kmer frequency distribution methods could provide reliable genome size estimates to predict repeat content and heterozygosity rates

1.  flow cytometry(流式细胞分析)

基因组大小指生物个体单倍体基因组所含的 DNA 总量。以基因组的碱基对来表示。每个细胞中以皮克(pg，10^-12g)水平表示。

C值是指真核生物细胞中，单倍细胞核（受精卵或二倍体体细胞中的一半量）里所拥有的DNA含量。

流式细胞仪的原理是通过发挥荧光染料定性定量与 DNA 双链碳架结构相结合的特性进行测量的。具体来看，荧光信号的强弱与DNA 含量正相关，结合的 DNA 越多，则引发的荧光强度也就越强。

> 样品细胞核DNA 含量/样品2C 峰的平均相对荧光强度=果蝇细胞核DNA 含量/果蝇2C 峰的平均相对荧光强度（相对荧光强度与DNA 含量呈正比，其中黑腹果蝇作为参照基因组，根据Gregory和Johnston（2008）的结果，其C 值为0.18 pg）。进而根据1 pg = 978 Mb，估算该物种的基因组大小。

2. kmer frequency distribution

从一段连续序列中迭代地选取长度为K个碱基的序列，若每条序列的长度为L，那么可以得到（L-K+1）个K-mer。

每次取k-mer是同一条reads正反取两次，这就是对这条reads的反向互补序列再取一次k-mer。

![K-mer](https://cdn.liguocheng.top//uPic/M8Kgmh.jpg)

- **评估基因组大小**

在数据量一定的情况下，K-mer出现的频数是服从泊松分布的，K-mer频率分布曲线的峰值作为其期望测序深度。基因组大小计算：G = Knum/Kdepth

通过将reads切割成以k为单位的k-mer，由于测序错误具有随机性，这些由于测序错误生成的k-mer绝大多数都是原测序物种中不存在的k-mer，因此都只出现了1次，要是将这些k-mer去掉，那么就会较大的可能除去测序错误，从而使得组装的结果更加可靠。

Kdepth=1的情况认为是错误情况，计算错误率，可以用于修正基因组大小。Revised Genome size= Genome size X (1-Error Rate)

- **杂合度、重复序列比例评估**

杂合峰：如果目标基因组有一定的杂合性，会在K-mer深度分布曲线主峰位置（c）的1/2处（c/2）出现一个小峰。同时杂合率越高，该峰越明显。

重复峰：如果在x=c处出现主峰，x=2c处有一个次峰，说明有一部分片段出现的期望值是大部分的2倍，这些片段为重复片段，次峰为重复峰。

![y1KEBx](https://cdn.liguocheng.top//uPic/y1KEBx.jpg)

- **物种样品污染**

通常评估物种DNA样品的污染程度有两种方法，一种是通过**k-mer深度分布曲线**，主要观看是否有着峰异常的情况；另一种是通过**GC含量分布图**，查看图中是否存在多个密度集中的类群。

> **k-mer只能是奇数**，就是为了防止通过k-mer组装时，**正反链混淆**。因为，当我们构建De Bruijn graph的时候，我们得准确把属于同一条read上的k-mer连接起来，我们不能一会儿把A kmer正确地连到它自己所在的read，一会儿又连到它互补链的read上去了！
>
> 一般来说，我们一般选用17-mer来估算基因组大小，因为ATCG四种不同的碱基组成长度为17的核苷酸有，足以覆盖一般大小的基因组。
>
> 当基因组中有较多的重复序列时，这时就可以使用较大的k-mer来跨过高重复的区域，从而获得更加准确及完整的基因组草图；由于reads上的碱基错误率的存在，选择较长的k-mer会带来较高的错误率，但这也可以加大测序深度来弥补。所以**为了得到更加完整的基因组，要尽可能使用较长的k-mer用于组装**。

> While most genome assemblers are haploid mode (some diploid-aware mode) to collapse allelic differences into one consensus sequence, using complex polyploid or less inbred diploid genomes can greatly increase the number of present alleles, which will likely result in a more fragmented assembly or create uncertainties about the contigs’ homology.

#### Is there high/low GC content in a genomic region?

二代Illumina在这些区域测序的覆盖度会很低，但三代PacBio and ONT不会产生这种bias

#### How many repetitive sequences (or transposable elements) will likely be present in the genome?

> To resolve the assembly of repeats (or if the subject genome has a high repeat content), using TGS reads that are sufficiently long to include the unique sequences flanking the repeats is an effective strategy.

### Step 3: Design the best experimental workflow

> While NGS is a useful tool for determining DNA sequences, certain parameters need to be considered prior to running an NGS experiment, such as quality control, SGS versus TGS, read length, read quality/error rate, number of reads, genome read coverage/depth, library preparation, and downstream applications.

No matter which assembly approaches and technologies are taken, genome assembly’s purpose is to construct a consensus haploid or haploid-phased chromosome-level assembly.

![Commonly used tools and programs for genome assembly.](https://cdn.liguocheng.top//uPic/MDsJlI.jpg)

### Step 4: Choose the best sequencing platforms and library preparations

`测序平台和文库构建直接相关`

Sample library preparation for WGS is dependent on two considerations: (1) the genome size of the target sample organism; and (2) the amount of sample available to be sequenced.

- short (Illumina, 454, SOLiD, and Ion Torrent), long (ONT and PacBio), or a combination (hybrid) read.

> while SGS technologies can produce high-throughput, fast, cheap, and highly accurate reads of lengths in the range 75 to 700 bp, they show limited ability to resolve complex regions with repetitive or heterozygous sequences, which results in incomplete or heavily fragmented genome assemblies.



> TGS technologies can produce long single-molecule reads (averaging >30 kb) with complete contiguity, facilitating assembly.



> Combining data from both SGS and TGS in a “hybrid approach/assembly” can compensate for the downsides of both approaches and is a cost-effective method because SGS data can correct errors in TGS reads.

- Additional techniques such as optical mapping (BioNano, San Diego, California, USA) and chromatin association (Hi-C) are highly recommended to facilitate contig joining and genome assembly completion.

### Step 5: Select the best possible DNA source and DNA extraction method

In general, the minimum DNA input is required for Illumina and 10xGC > 3 ng, PacBio > 20 μg, ONT > 1 μg, BioNano > 200 ng, and Dovetail > 5 μg.

> Attractive strategies include generating an inbred line of individuals for low-heterozygosity pooled sequencing and/or sequencing of haploid tissues as the foundation for filtering out paralogous sequence variants.

it is always worth investing time in getting high-quality DNA that will result in high-quality data and assembly to save time and money.

`Garbage in -> Garbage out`

### Step 6: Check the computational resources and requirements

As a general guide, the successful assembly of a moderately sized diploid genome (approximately 1 Gb) using software pipelines requires a minimum computing resource of **96 physical central processing unit (CPU) cores**, **1 TB of high-performance random-access memory (RAM)**, **3 TB of local storage**, and **10 TB of shared storage**.

### Step 7: Choose the best computational design and pipeline

Optimizing a computational design and securing sufficient computer resources are essential steps to succeed in a genome assembly and annotation project.

### Step 8: Assemble the genome

> the recommended tools herein are the TGS pipeline: PacBio/ONT read sequencing (remove all contaminated DNA; plastids/bacterial contamination), read quality assessment, evaluation, and filtering!assembly!error correction and polishing using SGS reads!assessment! chromosome-level assembly using BioNano and Hi-C data.

![Recommended flowchart for genome assembly and annotation.](https://cdn.liguocheng.top//uPic/MheYB9.jpg)

### Step 9: Check the assembly quality before annotation

- N50 is a widely used metric for assessing an assembly’s contiguity, which is defined by the length of the shortest contig for which longer and equal-length contigs cover at least 50% of the assembly. 

- NG50 resembles N50 except for the metric, which relates to the genome size rather than the assembly size. 

- NA50 and NGA50 are analogous to N50 and NG50 where the contigs are replaced by blocks aligned to the reference.

### Step 10: Genome annotation

(1) identifying noncoding regions; 

(2) identifying coding regions (called gene prediction); 

(3) attaching the biological information of these elements.

- In general, the identification of noncoding regions includes small and long sequences including repetitive and transposable elements.

![AvKTdQ](https://cdn.liguocheng.top//uPic/AvKTdQ.jpg)

![TtAEZG](https://cdn.liguocheng.top//uPic/TtAEZG.jpg)

Both ab initio and evidence-based prediction approaches are widely used as each approach has pros and cons. While **Augustus and SNAP** are the most popular tools for ab initio prediction, they still necessitate the information of the closely related gene and genome model for screening against the newly sequenced genome.

> evidence-based prediction usually uses results obtained by aligning ESTs, protein sequences, and RNA-seq data (results are even better with full-length Iso-Seq data from PacBio or ONT) to a genome assembly as external evidence. Trained gene predictors (training with Augustus and SNAP to obtain more accurate annotation results is highly recommended) can be used in **MAKER, BRAKER, and StringTie**.

- functional annotation—the process of attaching biological information to gene or protein sequences—must be performed. This can be carried out through homology search and gene ontology (GO) term mapping.

### Step 11: Build a searchable and sharable output format

(1) Do you want to share your data? (2) Do you have enough in-house expertise and infrastructure to maintain and improve the data, including data storage space? (3) Do you want to form internal and external collaborations to increase research productivity?

### Step 12: Reach out to the community to refine the assembly and annotation

Dropping whole-genome shotgun sequencing costs and improvements in bioinformatics pipelines and computer capabilities have resulted in the situation where a small lab can undertake genome projects (assembly and annotation), and any organism can become a model species.

## Advice for new genomic users to select a basic assembly and annotation pipeline

![xKwvYm](https://cdn.liguocheng.top//uPic/xKwvYm.jpg)

## Reference

https://doi.org/10.1371/journal.pcbi.1008325

https://cloud.tencent.com/developer/article/1613847

https://www.jianshu.com/p/f03789bd4d87

## License

Copyright 2021 [Guo-Cheng Li](https://moths.com). Released under the [MIT](https://github.com/gcushen/hugo-academic/blob/master/LICENSE.md) license.

