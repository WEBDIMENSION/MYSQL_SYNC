FROM vsamov/mysql-5.1.73

RUN apt update
RUN apt install -y \
    openssh-server \
    curl

# SSH key
RUN rm -rf /root/.ssh
RUN mkdir -m 700 /root/.ssh
RUN chown -R root:root /root/.ssh/
COPY ./id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

RUN #service mysql start
# Open ssh port
EXPOSE 22


