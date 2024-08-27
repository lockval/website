FROM floryn90/hugo:latest-ext
RUN wget https://go.dev/dl/go1.21.8.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.21.8.linux-amd64.tar.gz

ENV PATH=$PATH:/usr/local/go/bin

RUN git config --global --add safe.directory /workspaces/website


ENV LC_ALL C.UTF-8

