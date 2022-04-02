FROM ansible-base

RUN yum -y install ansible java-11-openjdk

COPY ansible /etc/ansible/

RUN ansible-playbook /etc/ansible/setup.yaml -v -e "aem_mode=author aem_environment=prd"