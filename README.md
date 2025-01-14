Aqui está o pipeline ajustado com um link para o tutorial de instalação difícil:

```markdown
# Pipeline de Análise de Dados

Este repositório contém instruções para instalação e execução de ferramentas bioinformáticas como **sickle** e **scythe**, bem como o processamento de dados de sequenciamento.

---

## 🛠️ Instalação

### Método Fácil (via apt)

```bash
sudo apt install sickle
sudo apt install scythe
```

### Método Difícil (Compilação Manual)

Caso deseje seguir o processo de instalação manual das ferramentas, consulte o [tutorial de instalação difícil](#tutorial-dificil).

---

## 📁 Tratamento de Dados

### 1. Baixar Sequências

```bash
# Comandos para baixar os arquivos de sequência
curl -O -J -L https://osf.io/shqpv/download
curl -O -J -L https://osf.io/9m3ch/download

# Verificar os arquivos baixados
ls -l
fastqc SRR957824_500K_R1.fastq.gz SRR957824_500K_R2.fastq.gz
```

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

```bash
megahit -1 SRR957824_trimmed_R1.fastq -2 SRR957824_trimmed_R2.fastq -o assemble.fasta
```

---

## 🔬 Análise de Metabólitos Secundários

- Ferramenta recomendada: [**AntiSMASH**](https://antismash.secondarymetabolites.org)

---

## 🔗 Tutorial de Instalação Difícil

```bash
# Criar diretório para armazenar softwares
mkdir software
cd software

# Instalar o sickle
git clone https://github.com/ucdavis-bioinformatics/sickle.git
cd sickle
make
./sickle

# Adicionar o caminho ao PATH
echo $PATH
pwd  # Exibir o caminho completo
export PATH=$PATH:<inserir_o_caminho_do_pwd>
cd ..

# Instalar o scythe
git clone https://github.com/ucdavis-bioinformatics/scythe.git
cd scythe
make
./scythe

# Adicionar o caminho ao PATH
echo $PATH
pwd
export PATH=$PATH:<inserir_o_caminho_do_pwd>
```

---

Se houver dúvidas ou melhorias, abra uma **issue** ou contribua com um **pull request**! 🚀
```
