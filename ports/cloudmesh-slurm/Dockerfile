FROM rockylinux:8

LABEL org.opencontainers.image.source="https://github.com/github/slurm-docker-cluster" \
      org.opencontainers.image.title="slurm-docker-cluster-with-cloudmesh" \
      org.opencontainers.image.description="Slurm Docker cluster on Rocky Linux 8 with cloudmesh" \
      org.label-schema.docker.cmd="docker-compose up -d" \
      maintainer="Gregor von Laszewski and JP. Fleischer"

# adapted from Giovanni Torres who created the template for slurm initially
# It was enhanced by installing
# * python 3.10.8
# * openmpi 4.1.4
# * cloudmesh-cmd5
# * cloudmesh-sbatch
# * cloudmesh-mpi
# * add of the PDF manual for python mpi developed by the cloudmesh team
# * https://github.com/cloudmesh/cloudmesh-mpi/blob/main/docs/report-mpi.pdf

ARG SLURM_TAG=slurm-21-08-6-1
# https://github.com/SchedMD/slurm/releases/tag/slurm-21-08-8-2
# ARG SLURM_TAG=slurm-21-08-8-2

ARG GOSU_VERSION=1.11

RUN set -ex \
    && yum makecache \
    && yum -y update \
    && yum -y install dnf-plugins-core \
    && yum config-manager --set-enabled powertools \
    && yum -y install \
       wget \
       bzip2 \
       perl \
       gcc \
       gcc-c++\
       git \
       gnupg \
       make \
       munge \
       munge-devel \
       mariadb-server \
       mariadb-devel \
       psmisc \
       bash-completion \
       vim-enhanced \
    && yum clean all \
    && rm -rf /var/cache/yum

# Python installation

ARG PYTHON=3.10.8

RUN set -ex \
    && yum groupinstall "Development Tools" -y \
    && yum install openssl-devel libffi-devel bzip2-devel -y \
    && cd ~ \
    && wget https://www.python.org/ftp/python/${PYTHON}/Python-${PYTHON}.tgz \
    && tar xvf Python-${PYTHON}.tgz \
    && cd Python-${PYTHON} \
    && ./configure --enable-optimizations \
    && make altinstall \
    && yum install python3-pip -y

RUN alternatives --set python /usr/bin/python3

RUN pip3 install Cython nose

RUN set -ex \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -rf "${GNUPGHOME}" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

RUN set -x \
    && git clone -b ${SLURM_TAG} --single-branch --depth=1 https://github.com/SchedMD/slurm.git \
    && pushd slurm \
    && ./configure --enable-debug --prefix=/usr --sysconfdir=/etc/slurm \
        --with-mysql_config=/usr/bin  --libdir=/usr/lib64 \
    && make install \
    && install -D -m644 etc/cgroup.conf.example /etc/slurm/cgroup.conf.example \
    && install -D -m644 etc/slurm.conf.example /etc/slurm/slurm.conf.example \
    && install -D -m644 etc/slurmdbd.conf.example /etc/slurm/slurmdbd.conf.example \
    && install -D -m644 contribs/slurm_completion_help/slurm_completion.sh /etc/profile.d/slurm_completion.sh \
    && popd \
    && rm -rf slurm \
    && groupadd -r --gid=990 slurm \
    && useradd -r -g slurm --uid=990 slurm \
    && mkdir /etc/sysconfig/slurm \
        /var/spool/slurmd \
        /var/run/slurmd \
        /var/run/slurmdbd \
        /var/lib/slurmd \
        /var/log/slurm \
        /data \
    && touch /var/lib/slurmd/node_state \
        /var/lib/slurmd/front_end_state \
        /var/lib/slurmd/job_state \
        /var/lib/slurmd/resv_state \
        /var/lib/slurmd/trigger_state \
        /var/lib/slurmd/assoc_mgr_state \
        /var/lib/slurmd/assoc_usage \
        /var/lib/slurmd/qos_usage \
        /var/lib/slurmd/fed_mgr_state \
    && chown -R slurm:slurm /var/*/slurm* \
    && /sbin/create-munge-key

COPY slurm.conf /etc/slurm/slurm.conf
COPY slurmdbd.conf /etc/slurm/slurmdbd.conf
RUN set -x \
    && chown slurm:slurm /etc/slurm/slurmdbd.conf \
    && chmod 600 /etc/slurm/slurmdbd.conf

RUN set -x \
    && yum install python3-devel -y

RUN set -x \
    && cd ~ \
    && wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.4.tar.gz \
    && gunzip -c openmpi-4.1.4.tar.gz | tar xf - \
    && cd openmpi-4.1.4 \
    && ./configure --prefix=/usr/local \
    && make all install

RUN set -x \
    && python -V \
    && python -m venv ~/ENV3 \
    && source ~/ENV3/bin/activate \
    && mkdir ~/cm \
    && cd ~/cm \
    && pip install mpi4py \
    && pip install cloudmesh-installer \
    && cloudmesh-installer get sbatch mpi

RUN set -x \
    && source ~/ENV3/bin/activate \
    && cms help

RUN set -x \
	&& cd ~ \
    && wget https://github.com/cloudmesh/cloudmesh-mpi/blob/main/docs/report-mpi.pdf

RUN set -x \
	&& cd ~ \
    && source ~/ENV3/bin/activate \
    && python -c "from mpi4py import MPI; print('mpi4py version:', MPI.Get_version())"

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]

CMD ["slurmdbd"]
