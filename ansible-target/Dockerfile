FROM ansible-base

RUN yum -y install ansible java-11-openjdk

COPY files/* /root/

RUN ansible-playbook /root/install_aem.yaml -v -e "aem_mode=author aem_environment=prd"