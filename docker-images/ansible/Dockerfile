
# Use the official image as a parent image
FROM ubuntu

# Update the system
RUN apt-get update 

# Install OpenSSH Server
RUN apt-get install -y openssh-server \
    && apt-get install -y net-tools \
    && apt-get install -y inetutils-ping \
    && apt-get install -y neofetch \
    && apt-get install -y vim \
    && apt install -y software-properties-common \
    && add-apt-repository --yes --update ppa:ansible/ansible \
    && apt install -y ansible

# Set up configuration for SSH
RUN mkdir /var/run/sshd
RUN echo 'root:root' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise, user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

RUN cd /etc/ansible
RUN ansible-config init --disabled > /etc/ansible/ansible.cfg 
RUN sed -i 's/;host_key_checking=True/host_key_checking=False/' /etc/ansible/ansible.cfg

# Expose the SSH port
EXPOSE 22   

# Run SSH
CMD ["/usr/sbin/sshd", "-D"]


