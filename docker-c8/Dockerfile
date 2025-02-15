FROM rockylinux:8

RUN dnf -y install zip curl

# Create all possible pool accounts
RUN curl -s https://raw.githubusercontent.com/stfc/grid-workernode/master/resources/pool_accounts/create.sh | bash -

# CVMFS (for SUM tests only; install before adding other repos to ensure we have the latest version)
RUN dnf -y install https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest.noarch.rpm \
                   https://download.opensuse.org/repositories/home:/cvmfs:/contrib-egi/CentOS_8/noarch/cvmfs-config-egi-2.6-1.7.obs.el8.noarch.rpm

# EPEL
RUN dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

# Basic dependencies
RUN dnf -y install freetype expat gcc glibc-headers time man unzip quota attr tcsh

# Boost (need to ensure gfal CLI from CVMFS will work for some VOs)
RUN dnf -y install boost-date-time boost-filesystem boost-graph boost-iostreams boost-program-options \
                   boost-regex boost-serialization boost-signals boost-system boost-test \
                   boost-thread boost-wave

# Singularity
RUN dnf -y install singularity

# Disable overlay and loop device management in singularity
RUN sed -i '/enable overlay/c\enable overlay = no' /etc/singularity/singularity.conf && \
    sed -i '/enable underlay/c\enable underlay = yes' /etc/singularity/singularity.conf && \
    sed -i '/max loop devices/c\max loop devices = 0' /etc/singularity/singularity.conf

# Deny setuid in Singularity
RUN sed -i '/allow setuid/c\allow setuid = no' /etc/singularity/singularity.conf

# Update & cleanup
RUN dnf -y update && \
    dnf clean all && \
    rm -rf /var/cache/dnf
