FROM jenkins/jenkins:lts
MAINTAINER bin.yang@intel.com

# cloudbees-folder - Folders
# htmlpublisher - HTML Publisher
# workflow-aggregator - Pipeline
# pipeline-github-lib - Pipeline: GitHub Groovy Libraries
# pipeline-stage-view - Pipeline: Stage View
# github-branch-source - GitHub Branch Source
# git - Git
# matrix-auth - Matrix Authorization Strategy
# pam-auth - PAM Authentication
# ldap - LDAP
# email-ext - Email Extension
# mailer - Mailer
# SSH Slaves - ssh-slaves
# Locale - locale
# extensible-choice-parameter - Extensible Choice Parameter
# gitlab-plugin - GitLab

RUN /usr/local/bin/install-plugins.sh \
	cloudbees-folder \
	htmlpublisher \
	workflow-aggregator \
	pipeline-github-lib \
	pipeline-stage-view \
	github-branch-source \
	git \
	matrix-auth \
	pam-auth \
	ldap \
	email-ext \
	mailer \
	ssh-slaves \
	jdk-tool \
	locale \
	extensible-choice-parameter \
	gitlab-plugin \
	kubernetes-plugin \
	kubernetes-pipeline-plugin
