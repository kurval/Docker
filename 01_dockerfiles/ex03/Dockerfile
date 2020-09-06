FROM debian

EXPOSE 443 22

RUN apt-get update -y \
&& apt-get upgrade -y \
&& apt-get install -y curl openssh-server ca-certificates

# GitLab repo
RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash \
&& apt-get install -y gitlab-ce

# Self signed certificate
RUN mkdir -p /etc/gitlab/ssl \
&& chmod 755 /etc/gitlab/ssl \
&& openssl req -new -x509 -nodes -batch -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt -days 365 \
&& echo "external_url \"https://$(docker-machine ip Char)\"" >> /etc/gitlab/gitlab.rb \
&& echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/gitlab.crt\"" >> /etc/gitlab/gitlab.rb \
&& echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/gitlab.key\"" >> /etc/gitlab/gitlab.rb \
&& echo "gitlab_rails['gitlab_shell_ssh_port'] = 22" >> /etc/gitlab/gitlab.rb

ENTRYPOINT  (/opt/gitlab/embedded/bin/runsvdir-start &) \
&& service ssh start \
&& gitlab-ctl reconfigure \
&& gitlab-ctl tail

# How to build ?
# docker build -t gitlab .

# How to run ?
# docker run --rm -it -p 8443:443 -p 8022:22 --privileged gitlab

# Tests :
# https://<docker-machine ip Char>:8443
# The reset password page (first page) creates a user 'root' with the password you entered

# 2) HTTPS :
# clone : env GIT_SSL_NO_VERIFY=true git clone https://<docker-machine ip Char>/root/<repo>.git
# push : env GIT_SSL_NO_VERIFY=true git push

# 3) SSH :
# Add your ssh key in gitlab:
# ssh-keygen -t ed25519 -C 'root@gitlab.com' -f id_ed25519
# pbcopy < .ssh/id_ed25519.pub
# ssh -T -i id_ed25519 -p 8022 git@<docker-machine ip Char>

# clone : env GIT_SSH_COMMAND='ssh -i id_ed25519 -p 8022' git clone git@<docker-machine ip Char>:root/<repo>.git
# push : env GIT_SSH_COMMAND='ssh -i ../id_ed25519 -p 8022' git push

# https://medium.com/@1997corry/running-your-own-gitlab-on-docker-community-edition-39c4a8f99553
