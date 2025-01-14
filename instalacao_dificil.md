# Tutorial de Instalação Difícil

## Passos

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
