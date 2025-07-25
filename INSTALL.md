# Install

The definitive steps to install polymarker, at least on almalinux 9.


```bash
sudo dnf group install -y "Development Tools"
mkdir sw
cd sw
wget https://mafft.cbrc.jp/alignment/software/mafft-7.526-gcc_fc6.x86_64.rpm
sudo dnf install wget
wget https://mafft.cbrc.jp/alignment/software/mafft-7.526-gcc_fc6.x86_64.rpm
sudo dnf install -y ./mafft-7.526-gcc_fc6.x86_64.rpm
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.16.0/ncbi-blast-2.16.0+-1.x86_64.rpm
sudo dnf install -y ./ncbi-blast-2.16.0+-1.x86_64.rpm 
wget https://github.com/primer3-org/primer3/archive/refs/tags/v2.6.1.tar.gz
tar -xf v2.6.1.tar.gz 
cd primer3-2.6.1/src/
make
sudo make install
cd ../..
sudo dnf install -y glib2-devel
wget https://github.com/cb2e6f/exonerate/archive/refs/tags/v2.4.0.tar.gz
tar -xf v2.4.0.tar.gz 
cd exonerate-2.4.0/
./configure
make
sudo make install
cd ..
wget https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2
tar -xf samtools-1.21.tar.bz2 
cd samtools-1.21/
sudo dnf install -y ncurses-devel
sudo dnf install -y bzip2-devel
./configure 
make
sudo make install
cd ../..
sudo dnf install -y ruby
sudo dnf install -y ruby-devel
sudo gem install --no-user-install rake
sudo gem install --no-user-install sorted_set
sudo gem install --no-user-install bio-polymarker
```
