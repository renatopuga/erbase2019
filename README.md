#ERBASE 2019 - XIX Escola Regional de Computação Bahia - Alagoas - Sergipe

```
Minicurso - Bioinformática Aplicada à área da Saúde
Ministrado por: Renato Puga
Horário       : 09h00 - 15h30
E-mail        : renatopuga @ gmail . com 
```
[Acesse o site ERBASE2019](https://erbase2019.tecnojr.com.br/)

## Sobre o Minicurso

Utilizando a plataforma **Galaxy Hub** e seguindo o pipeline de chamada de variantes genéticas, nós partimos dos dados brutos (.FASTQ), geramos **Relatórios de Qualidade** com os programas ([FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) e [MultiQC](https://multiqc.info/)), mapeameamos as sequências no genoma de referência com ([BWA-MEM](https://github.com/lh3/bwa)) e finalizamos chamando variantes com o ([FreeBayes](https://github.com/ekg/freebayes)) até chegarmos ao arquivo de variantes (.VCF).

Referência: [Tutorial NGS] (https://galaxyproject.org/tutorials/ngs/)

## Roteiro - usegalaxy

* [Aula Introdução - Clique aqui para acessar os Slides](https://www.slideshare.net/RenatoPuga/erbase-2019-renato-puga/RenatoPuga/erbase-2019-renato-puga)
* Prática com use Galaxy (roteiro abaixo)

## Prática

### Manipulando dados de NGS com o Galaxy
Nesta seção, veremos aspectos práticos da manipulação de dados de sequenciamento de próxima geração. Começaremos com o formato FASTQ produzido pela maioria das máquinas de sequenciamento e terminaremos com o formato SAM / BAM representando as leituras mapeadas.


### Criando uma conta no Galaxy

Acesse: [usegalaxy.org] (https://usegalaxy.org/)

Clique no Menu: ***Login or Register > Register***:

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-register.png?raw=true">


**Fonte:** Galaxy Login or Register. ([usegalaxy] (usegalaxy.org))

### Carregando Arquivos no Galaxy

Copie os links abaixo e siga os passos para carregar os dados na plataforma Galaxy:

```
https://zenodo.org/record/583613/files/sample1-f.fq.gz
https://zenodo.org/record/583613/files/sample1-r.fq.gz
https://zenodo.org/record/583613/files/sample2-f.fq.gz
https://zenodo.org/record/583613/files/sample2-r.fq.gz
```

Agora, no Galaxy, clique no ícone <img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-load-data.png?raw=true"> para abrir a janela ***Download from web or upload from disk*** , então clique no botão ***Paste/Fetch data*** e cole os links das amostras no painel.

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-upload-fastq.png?raw=true">


### FASTQ - Manipulação e ***Quality Control*** (QC)

Os dados são paired end data (arquivos com -f são ***forward*** e arquivos com -r são ***reverses***) representando duas amostras independentes produzidas por uma máquina Illumina.

#### Formado FASTQ 

O FastQ não é um formato muito bem definido. No início, vários fabricantes de instrumentos de sequenciamento estavam livres para interpretar o FASTQ, resultando em múltipas variações. Essa variação resultou principalmente de diferentes maneiras de codificar os valores de qualidade, conforme descrito aqui (abaixo, você explicará os índices de qualidade e seu significado). 

Hoje, a versão FASTQ do Sanger é considerada a forma padrão de FASTQ. O Galaxy está usando o fastq sanger como a única entrada legítima para ferramentas de processamento downstream e fornece vários utilitários para converter arquivos fastq nesse formato (consulte NGS: QC e seção de manipulação de ferramentas do Galaxy).

#### Estrutura FASTQ

```
@M02286:19:000000000-AA549:1:1101:12677:1273 1:N:0:23
CCTACGGGTGGCAGCAGTGAGGAATATTGGTCAATGGACGGAAGTCTGAACCAGCCAAGTAGCGTGCAG
+
ABC8C,:@F:CE8,B-,C,-6-9-C,CE9-CC--C-<-C++,,+;CE<,,CD,CEFC,@E9<FCFCF?9
@M02286:19:000000000-AA549:1:1101:15048:1299 1:N:0:23
CCTACGGGTGGCTGCAGTGAGGAATATTGGACAATGGTCGGAAGACTGATCCAGCCATGCCGCGTGCAG
+
ABC@CC77CFCEG;F9<F89<9--C,CE,--C-6C-,CE:++7:,CF<,CEF,CFGGD8FFCFCFEGCF
@M02286:19:000000000-AA549:1:1101:11116:1322 1:N:0:23
CCTACGGGAGGCAGCAGTAGGGAATCTTCGGCAATGGACGGAAGTCTGACCGAGCAACGCCGCGTGAGT
+
AAC<CCF+@@>CC,C9,F9C9@9-CFFFE@7@:+CC8-C@:7,@EFE,6CF:+8F7EFEEF@EGGGEEE
```

-  Cada sequência é representada por quatro linhas:
	1. @ sequida pela ID da sequência e informações da corrida são opcionais
	2. sequência de nucleotído
	3. + (separador)
	4. score de qualidade de cada base codificada com os símbolos da tabela ASCII
		
### Manipulando arquivos FASTQ 

Um dos primeiros passos na análise dos dados de NGS é verificar a qualidade dos dados. O FastQC é uma ferramenta que permite avaliar a qualidade dos conjuntos de dados fastq.

Utilize a busca para encontrar o programa FASTQC: 
***NGS: QC and manipulation > FastQC Read Quality reports***.

Agora, clique no ícone ***Multiple datasets*** e selecione todos os arquivos **.fq.gz**.

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-fastqc-run.png?raw=true">

#### MultiQC Report

Uma ferramenta modular para agregar resultados de análises de bioinformática em muitas amostras em um único relatório.

Utilize a busca para encontrar o programa MultiQC: 
***NGS: QC and manipulation > MultiQC aggregate results from bioinformatics analyses into a single report***.

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-multiqc-run.png?raw=true">

### Mepeamento

#### Mapeando seus dados

O mapeamento de sequências de NGS contra sequências de referência é uma das principais etapas da análise. Em nosso exemplo vamos utilizar o algorítmo BWA-mem:

* BWA-MEM 2013 - Li

#### Mapeamento (genoma pré-computado)

Os mapeadores geralmente comparam as sequências com uma sequência de referência que foi transformada em uma estrutura de dados altamente acessível chamada índice do genoma. Tais índices devem ser gerados antes do início do mapeamento. As instâncias do Galaxy normalmente armazenam índices para várias construções de genoma disponíveis publicamente.

* bwa-mem
* hg19
* paired-end reads

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-bwa-run.png?raw=true">

### Call Variants

FreeBayes bayesian genetic variant detector.
***NGS: Variant Analysis > FreeBayes bayesian genetic variant detector***

<img src="https://github.com/renatopuga/erbase2019/blob/master/img/galaxy-freebayes-run.png?raw=true">


