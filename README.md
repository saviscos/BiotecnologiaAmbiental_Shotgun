# Aula - Biotecnologia ambiental - Analises de Metagenomas.

Este repositório contém instruções para instalação e execução de ferramentas como **sickle** e **scythe**  
Bem como o processamento de dados de sequenciamento.

---


## Metagenoma

**Metagenoma** é o conjunto completo de material genético recuperado diretamente de amostras ambientais, sem a necessidade de cultivo dos micro-organismos presentes. A análise metagenômica permite estudar comunidades microbianas complexas, identificando tanto as espécies presentes quanto suas funções biológicas.

### Exemplos de Aplicação:
- Estudos de microbiomas (intestinal, marinho, solo).
- Descoberta de novas enzimas e compostos bioativos.
- Análise de resistência antimicrobiana e patógenos emergentes.


![Metagenoma](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/Screenshot%202025-01-14%20at%2011-26-50%20Metagenomics%20Principle%20Types%20Steps%20Uses%20Examples%20Diagram.png)



## 🛠️ Instalação

### Método Fácil (via apt)

```bash
sudo apt install sickle
sudo apt install scythe
```


### Método Difícil (Compilação Manual)

Caso deseje seguir o processo de instalação manual das ferramentas, consulte o [Tutorial de Instalação Difícil](instalacao_dificil.md).



## 📁 Tratamento de Dados

### 1. Baixar Sequências

# Comandos para baixar os arquivos de sequência

```bash
curl -O -J -L https://osf.io/shqpv/download
curl -O -J -L https://osf.io/9m3ch/download
```
# Verificar os arquivos baixados

```bash
ls -l
fastqc SRR957824_500K_R1.fastq.gz SRR957824_500K_R2.fastq.gz
```

## Tipos de Sequenciadores: Illumina vs Nanopore

### Illumina (Sequenciamento de Segunda Geração)
- **Princípio**: Sequenciamento por síntese (SBS), com nucleotídeos fluorescentes.
- **Tipo de Leitura**: Curtas (50-300 pares de bases).
- **Precisão**: Alta precisão (>99,9%).
- **Vantagens**: Alta cobertura, baixo custo por base.
- **Desvantagens**: Requer fragmentação do DNA em pequenos pedaços.
- **Exemplo de Aplicação**: RNA-seq, análise de variantes, genomas bacterianos.

### Nanopore (Sequenciamento de Terceira Geração)
- **Princípio**: Sequenciamento pelo fluxo de íons através de um poro.
- **Tipo de Leitura**: Longas (até milhões de pares de bases).
- **Precisão**: ~95-98%.
- **Vantagens**: Leitura de moléculas inteiras de DNA/RNA e tempo real.
- **Desvantagens**: Maior taxa de erro.
- **Exemplo de Aplicação**: Montagem de novo, variantes estruturais, RNA de transcrição completa.

### Comparação Resumida:

| Característica  | Illumina            | Nanopore            |
|-----------------|---------------------|--------------------|
| Tipo de leitura | Curta (50-300 pb)    | Longa (Kb até Mb)  |
| Precisão        | >99,9%               | 95-98%             |
| Custo por base  | Baixo                | Moderado           |
| Tempo de leitura| Offline              | Tempo real         |
| Aplicação       | RNA-seq, pequenos genomas | Genomas completos e variantes estruturais |


### Exemplo de arquivo FASTq:

![Exemplo de FASTQ](https://raw.githubusercontent.com/saviscos/BiotecnologiaAmbiental_Shotgun/main/fastq_fig.jpg)

### Qualidade Phred:

Qualidade Phred é uma métrica usada para avaliar a precisão das bases chamadas durante o sequenciamento de DNA. Cada valor Phred (Q) representa a probabilidade de erro na identificação de uma base nucleotídica. A escala é logarítmica e quanto maior o valor, menor a chance de erro.

![Exemplo de Phred](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/phred_table.png)

### Tipos de Leituras: Single-End vs Paired-End

- **Single-End (Leitura Única)**: O sequenciamento é realizado em apenas uma extremidade do fragmento de DNA, gerando uma única leitura contínua. Este método é mais rápido, simples e barato, mas pode ser menos preciso em regiões repetitivas ou complexas do genoma.

- **Paired-End (Leitura Pareada)**: O sequenciamento é realizado nas duas extremidades do fragmento de DNA, gerando duas leituras com uma distância conhecida entre elas. Este método melhora a precisão e ajuda na montagem de genomas, principalmente em regiões com repetições ou lacunas.


![Sequenciamento](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/SE_vs_PE.png)



📊 **Resultados:**
- [Relatório FastQC R1](https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_500K_R1_fastqc.html)
- [Relatório FastQC R2](https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_500K_R2_fastqc.html)

### 2. Baixar Adaptadores

```bash
curl -O -J -L https://osf.io/v24pt/download
```

### 3. Remoção de Adaptadores

```bash
# Executar scythe para remover adaptadores
scythe -a adapters.fasta -o SRR957824_adapt_R1.fastq SRR957824_500K_R1.fastq.gz
scythe -a adapters.fasta -o SRR957824_adapt_R2.fastq SRR957824_500K_R2.fastq.gz
```

### 4. Filtragem de Leituras de Baixa Qualidade

```bash
sickle pe -f SRR957824_adapt_R1.fastq -r SRR957824_adapt_R2.fastq \
    -t sanger -o SRR957824_trimmed_R1.fastq -p SRR957824_trimmed_R2.fastq \
    -s /dev/null -q 25
```

📊 **Resultados:**
- [Relatório Trimmed R1](https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_trimmed_R1_fastqc.html)
- [Relatório Trimmed R2](https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_trimmed_R2_fastqc.html)

---

## 🧬 Montagem

Montagem de genomas:

Montagem de genomas é o processo de reconstrução da sequência completa do DNA de um organismo a partir de pequenos fragmentos gerados pelo sequenciamento. 
O objetivo é unir os fragmentos em uma ordem correta, formando contigs e, posteriormente, scaffolds, até se obter uma representação contínua e o mais próxima possível do genoma original. Existem dois tipos principais de montagem: de novo (sem referência) e baseada em referência. 
A montagem de novo é usada quando não existe um genoma conhecido semelhante, enquanto a montagem baseada em referência utiliza um genoma pré-existente como guia.

![Montagem](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/hq720.jpg)



```bash
conda install -c bioconda megahit

megahit -1 SRR957824_trimmed_R1.fastq -2 SRR957824_trimmed_R2.fastq -o assemble.fasta
```

---

## 🔬 Análise de Metabólitos Secundários



Metabolitos Secundarios:

Metabólitos secundários são compostos orgânicos produzidos por organismos como bactérias, fungos e plantas que não estão diretamente envolvidos em seu crescimento, desenvolvimento ou reprodução, mas desempenham funções ecológicas importantes. 
Esses compostos podem atuar como mecanismos de defesa, sinalização celular ou como armas contra competidores. 
Exemplos incluem antibióticos, alcaloides, terpenos e flavonoides.


![Metabolitos](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/68747470733a2f2f692e6962622e636f2f466d42666d48572f6267632d6763662d696c6c757374726174696f6e2e706e67.png)


- Ferramenta recomendada: [**AntiSMASH**](https://antismash.secondarymetabolites.org)

---

```bash
conda activate antismash
```
