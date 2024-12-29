FROM redhat/ubi9

SHELL ["/bin/bash", "-c"]

RUN dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
RUN dnf install https://dev.mysql.com/get/mysql80-community-release-el9-4.noarch.rpm -y

RUN echo $'[ghettoforge] \n\
name     = ghettoforge \n\
baseurl  = https://mirror.ghettoforge.org/distributions/gf/el/9/gf/$basearch/ \n\
enabled  = 1 \n\
gpgkey   = https://mirror.ghettoforge.org/distributions/gf/RPM-GPG-KEY-gf.el9 \n\
gpgcheck = 1' > etc/yum.repos.d/ghettoforge.repo

RUN echo $'[opennebula] \n\
name     = opennebula \n\
baseurl  = https://downloads.opennebula.io/repo/6.10/RedHat/9/$basearch/ \n\
enabled  = 1 \n\
gpgcheck = 0' > etc/yum.repos.d/opennebula.repo

RUN echo $'[appstream] \n\
name      = CentOS Stream $releasever - AppStream \n\
#metalink = https://mirrors.centos.org/metalink?repo=centos-appstream-$stream&arch=$basearch&protocol=https,http \n\
baseurl   = https://mirror.stream.centos.org/9-stream/AppStream/x86_64/os \n\
enabled   = 1 \n\
gpgcheck  = 0' > etc/yum.repos.d/centos9-appstream.repo

RUN dnf install xmlrpc-c freerdp-libs net-tools qemu-img mysql-community-client genisoimage opennebula opennebula-sunstone opennebula-rubygems opennebula-fireedge opennebula-gate opennebula-flow opennebula-guacd rubygem-json -y

RUN mkdir -p /root/.one && echo "oneadmin:opennebula" > /root/.one/one_auth
RUN echo 'IM_MAD = [ NAME="dummy", SUNSTONE_NAME="Dummy", EXECUTABLE="one_im_sh", ARGUMENTS="-r 3 -t 15 -w 90 dummy"]' >> /etc/one/monitord.conf
RUN echo 'VM_MAD = [ NAME="dummy", SUNSTONE_NAME="Testing", EXECUTABLE="one_vmm_dummy",TYPE="xml" ]' >> /etc/one/oned.conf

EXPOSE 9869
EXPOSE 2633
EXPOSE 2474
