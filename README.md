# Pipeline de Análise de Dados

Este repositório contém instruções para instalação e execução de ferramentas como **sickle** e **scythe**  
Bem como o processamento de dados de sequenciamento.

---


Tipos de leituras:

![Metagenoma](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/Screenshot%202025-01-14%20at%2011-26-50%20Metagenomics%20Principle%20Types%20Steps%20Uses%20Examples%20Diagram.png)



## 🛠️ Instalação

### Método Fácil (via apt)

```bash
sudo apt install sickle
sudo apt install scythe
```


```markdown
### Método Difícil (Compilação Manual)

Caso deseje seguir o processo de instalação manual das ferramentas, consulte o [Tutorial de Instalação Difícil](instalacao_dificil.md).



---

## 📁 Tratamento de Dados

### 1. Baixar Sequências
```

```bash
# Comandos para baixar os arquivos de sequência
curl -O -J -L https://osf.io/shqpv/download
curl -O -J -L https://osf.io/9m3ch/download

# Verificar os arquivos baixados
ls -l
fastqc SRR957824_500K_R1.fastq.gz SRR957824_500K_R2.fastq.gz
```
Sequenciadores:

![Sequenciadores](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/3rd-gen-sequencing.png)


Exemplo de arquivo FASTq

![Exemplo de FASTQ](https://raw.githubusercontent.com/saviscos/BiotecnologiaAmbiental_Shotgun/main/fastq_fig.jpg)

Qualidade Phred:

![Exemplo de Phred](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/phred_table.png)

Tipos de leituras:

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

Tipos de leituras:

![Montagem](https://github.com/saviscos/BiotecnologiaAmbiental_Shotgun/blob/main/hq720.jpg)



```bash
megahit -1 SRR957824_trimmed_R1.fastq -2 SRR957824_trimmed_R2.fastq -o assemble.fasta
```

---

## 🔬 Análise de Metabólitos Secundários

- Ferramenta recomendada: [**AntiSMASH**](https://antismash.secondarymetabolites.org)

---

