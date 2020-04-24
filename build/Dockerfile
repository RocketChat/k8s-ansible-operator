FROM quay.io/operator-framework/ansible-operator:v0.16.0

LABEL name="Rocket.Chat Operator" \
      vendor="Rocket.Chat" \
      version="0.0.1" \
      release="1" \
      summary="The Ultimate Open Source Web Chat Platform Operator" \
      description="Deploys Rocket.Chat onto OpenShift" 

COPY LICENSE licenses/

COPY requirements.yml ${HOME}/requirements.yml
RUN ansible-galaxy collection install -r ${HOME}/requirements.yml \
 && chmod -R ug+rwx ${HOME}/.ansible

COPY watches.yaml ${HOME}/watches.yaml

COPY rocketchat.yml ${HOME}/rocketchat.yml

COPY roles/ ${HOME}/roles/
