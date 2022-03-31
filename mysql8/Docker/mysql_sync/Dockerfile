FROM mysql:8

RUN apt update
RUN apt install -y \
    openssh-server \
    curl

# SSH key
RUN rm -rf /root/.ssh
RUN mkdir -m 700 /root/.ssh
RUN chown -R root:root /root/.ssh/
COPY private_key /root/.ssh/
RUN chmod 600 /root/.ssh/private_key

# Open ssh port
EXPOSE 22


