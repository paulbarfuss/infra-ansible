FROM registry.fedoraproject.org/fedora:32

LABEL maintainers="Red Hat Communities of Practice" \
      usage="This image is meant to be used with the 'docker run'"

USER root

ENV OC_CLIENT_MIRROR=https://mirror.openshift.com/pub/openshift-v4/clients/ocp/stable-4.5/openshift-client-linux-4.5.9.tar.gz \
    USER_ID=1001 \
    WORK_DIR=/infra-ansible

COPY install-packages .

RUN dnf makecache && \
    dnf install -y --nodocs $(<install-packages) && \
    rm install-packages && \
    curl $OC_CLIENT_MIRROR | tar -C /usr/local/bin/ -xzf - && \
    alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
    dnf clean all && \
    rm -rf /var/cache/dnf

COPY root /

RUN /usr/local/bin/user_setup && \
    git clone https://github.com/redhat-cop/infra-ansible.git

USER ${USER_ID}

WORKDIR $WORK_DIR

ENTRYPOINT ["/usr/local/bin/entrypoint"]
CMD ["/usr/local/bin/run"]
