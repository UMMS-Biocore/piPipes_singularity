BootStrap: shub
From: shub://onuryukselen/singularity

%labels

    AUTHOR Onur Yukselen <onur.yukselen@umassmed.edu>
    Version v1.0

%environment
    PATH=$PATH:/Software/piPipes/bin:/Software/brew/bin
    export PATH

%apprun R
  exec R "$@"

%apprun Rscript
  exec Rscript "$@"

%runscript
  exec R "$@"  
  
%post

############
### piPipes
############
    mkdir -p /Software 
    cd /Software
    chmod 777 /Software
    git clone https://github.com/bowhan/piPipes.git /Software/piPipes
    cd /Software/piPipes
    ln -s $PWD/piPipes /usr/local/bin/piPipes
    ln -s $PWD/piPipes_debug /usr/local/bin/piPipes_debug
    
#### 1. R
  NPROCS=`awk '/^processor/ {s+=1}; END{print s}' /proc/cpuinfo`
  cd /tmp
  wget https://cran.rstudio.com/src/base/R-3/R-3.4.3.tar.gz
  tar xvf R-3.4.3.tar.gz
  cd /tmp/R-3.4.3
  apt-get install -y libblas3 libblas-dev liblapack-dev liblapack3  
  apt-get install -y libgmp10 libgmp-dev
  apt-get install -y fort77 aptitude
  aptitude install -y xorg-dev
  aptitude install -y libreadline-dev
  apt install -y   libpcre3-dev liblzma-dev  
  apt-get update

  ./configure --enable-R-static-lib --with-blas --with-lapack --enable-R-shlib=yes 
  echo "Will use make with $NPROCS cores."
  make -j${NPROCS}
  make install

  echo install.packages\(\"stringi\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"RColorBrewer\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"ggplot2\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"grid\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"ggthemes\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"gplots\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"parallel\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"scales\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"reshape\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"gridExtra\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"gdata\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"labeling\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"RCircos\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  echo install.packages\(\"reshape2\"\, dependencies = TRUE, repos\=\'https://cloud.r-project.org/\'\, Ncpus\=${NPROCS}\) | R --slave
  yes | apt-get install libmariadb-client-lgpl-dev
  R --slave -e "source('https://bioconductor.org/biocLite.R'); biocLite()"
  R --slave -e "source('https://bioconductor.org/biocLite.R'); biocLite('cummeRbund')"
  
  
  

# 2. HTSeq-count
pip install HTSeq
which htseq-count

# 3. MACS2
pip install macs2
which macs2

# 4. Perl Module Statistics::Descriptive;
cpan Statistics::Descriptive
perl -MStatistics::Descriptive -e "print \"Installed.\\n\";"

# 5. Install Gawk with Linuxbrew  
locale-gen "en_US.UTF-8"
dpkg-reconfigure locales
export LANGUAGE="en_US.UTF-8"
echo 'LANGUAGE="en_US.UTF-8"' >> /etc/default/locale
echo 'LC_ALL="en_US.UTF-8"' >> /etc/default/locale
cd /Software
chmod 777 /tmp
chmod +t /tmp
apt-get install -y apt-transport-https build-essential libsm6 libxrender1 libfontconfig1 ruby
useradd -m singularity
su -c 'cd /Software && git clone https://github.com/Linuxbrew/brew.git /Software/brew' singularity
su -c '/Software/brew/bin/brew install gawk' singularity
ln -s /Software/brew/bin/gawk /Software/piPipes/bin/awk
    
    
    
    
    
    
    
    