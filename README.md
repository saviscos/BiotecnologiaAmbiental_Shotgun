# BiotecnologiaAmbiental_Shotgun

Instalação dificil:


mkdir software
cd software
git clone https://github.com/ucdavis-bioinformatics/sickle.git
make

./sickle

echo $PATH

pwd

export PATH=$PATH:<add the path from the pwd command here>


mkdir software
cd software
git clone https://github.com/ucdavis-bioinformatics/scythe
make

./scythe

echo $PATH

pwd

export PATH=$PATH:<add the path from the pwd command here>


Instalação facil:


sudo apt install sickle

sudo apt install scythe



Tratamento de dados:


Baixar sequencias:

curl -O -J -L https://osf.io/shqpv/download
curl -O -J -L https://osf.io/9m3ch/download
ls -l
fastqc SRR957824_500K_R1.fastq.gz SRR957824_500K_R2.fastq.gz

Resultados: 

https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_500K_R1_fastqc.html

https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_500K_R2_fastqc.html
Baixar adaptadores:

curl -O -J -L https://osf.io/v24pt/download

scythe -a adapters.fasta -o SRR957824_adapt_R1.fastq SRR957824_500K_R1.fastq.gz
scythe -a adapters.fasta -o SRR957824_adapt_R2.fastq SRR957824_500K_R2.fastq.gz


sickle pe -f SRR957824_adapt_R1.fastq -r SRR957824_adapt_R2.fastq \
    -t sanger -o SRR957824_trimmed_R1.fastq -p SRR957824_trimmed_R2.fastq \
    -s /dev/null -q 25


Resultados: 

https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_trimmed_R1_fastqc.html

https://www.hadriengourle.com/tutorials/data/fastqc/SRR957824_trimmed_R2_fastqc.html


