ARG BASEOS_DIGEST
FROM quay.io/centos/centos:stream9${BASEOS_DIGEST:-}

SHELL ["/bin/bash", "-c"]

RUN echo $'[opennebula] \n\
name     = opennebula \n\
baseurl  = https://downloads.opennebula.io/repo/6.10/RedHat/9/$basearch/ \n\
enabled  = 1 \n\
gpgcheck = 0' > etc/yum.repos.d/opennebula.repo

RUN dnf config-manager --set-enabled crb && \
    dnf install epel-release https://dev.mysql.com/get/mysql80-community-release-el9-4.noarch.rpm net-tools -y

RUN dnf install opennebula opennebula-sunstone opennebula-node-kvm opennebula-rubygems opennebula-fireedge opennebula-gate opennebula-flow opennebula-guacd -y && \
    rm -Rf /usr/share/doc && rm -Rf /usr/share/man && dnf clean all

# RUN curl -O https://github.com/OpenNebula/addon-appmarket/releases/download/release-2.1.0/appmarket_2.1.0.rpm && dnf localinstall appmarket_2.1.0.rpm -y

RUN mkdir -p /root/.one && echo "oneadmin:opennebula" > /root/.one/one_auth && \
    echo 'IM_MAD = [ NAME="dummy", SUNSTONE_NAME="Dummy", EXECUTABLE="one_im_sh", ARGUMENTS="-r 3 -t 15 -w 90 dummy"]' >> /etc/one/monitord.conf && \
    echo 'VM_MAD = [ NAME="dummy", SUNSTONE_NAME="Testing", EXECUTABLE="one_vmm_dummy",TYPE="xml" ]' >> /etc/one/oned.conf && \
    sed -i -e "s/127.0.0.1/0.0.0.0/g" /etc/one/oneflow-server.conf

EXPOSE 9869
EXPOSE 2633
EXPOSE 2474
EXPOSE 2616
