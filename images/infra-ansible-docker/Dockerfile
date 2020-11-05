FROM registry.fedoraproject.org/fedora:32

LABEL maintainers="Red Hat Communities of Practice" \
      usage="This image is meant to be used with the 'docker run'"

USER root

ENV OC_CLIENT_MIRROR=https://mirror.openshift.com/pub/openshift-v4/clients/ocp/stable-4.5/openshift-client-linux-4.5.9.tar.gz \
    USER_ID=1001 \
    WORK_DIR=/infra-ansible

COPY install-packages .

RUN dnf makecache && \
    dnf install -y --nodocs \
      ansible \
      git \
      httpd-tools \
      java-1.8.0-openjdk-headless \
      jq \
      openssl \
      pandoc \
      python-boto3 \
      python-cinderclient \
      python-dns \
      python-glanceclient \
      python-heatclient \
      python-neutronclient \
      python-novaclient \
      python-openstackclient \
      python-openstacksdk \
      python-ovirt-engine-sdk4 \
      python-pip \
      python-saharaclient \
      python-setuptools \
      python-shade \
      python-swiftclient \
      python-troveclient && \
    curl $OC_CLIENT_MIRROR | tar -C /usr/local/bin/ -xzf - && \
    alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
    dnf clean all && \
    rm -rf /var/cache/dnf

COPY root /

RUN /usr/local/bin/user_setup && \
    git clone --single-branch --branch modernize_python2_dependencies https://github.com/paulbarfuss/infra-ansible.git

USER ${USER_ID}

WORKDIR $WORK_DIR

ENTRYPOINT ["/usr/local/bin/entrypoint"]
CMD ["/usr/local/bin/run"]
