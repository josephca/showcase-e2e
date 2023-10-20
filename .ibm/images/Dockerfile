FROM golang:1.19

ENV HELM_VERSION v3.13.1

# Install dependencies
# RUN apk add --no-cache curl tar gzip

# Install Helm
RUN curl -L "https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz" | tar zx \
    && mv linux-amd64/helm /usr/bin/ \
    && rm -rf linux-amd64

# Cleanup
# RUN apk del curl tar gzip

CMD ["helm"]

RUN curl -fsSL https://clis.cloud.ibm.com/install/linux | sh && \
    curl -sLO https://github.com/cli/cli/releases/download/v2.1.0/gh_2.1.0_linux_amd64.deb && \
    apt install ./gh_2.1.0_linux_amd64.deb && \
    curl -sLO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/ && \
    curl -sLO https://raw.githubusercontent.com/cptmorgan-rh/install-oc-tools/master/install-oc-tools.sh > /dev/null && \
    chmod +x install-oc-tools.sh && \
    ./install-oc-tools.sh --latest 4.11 && \
    apt-get update -y && \
    apt-get install -y sshpass jq colorized-logs && \
    rm -rf /var/lib/apt/lists/*

# Create non-root user and associated home directory
RUN useradd -u 2001 --create-home tester
# Change to non-root privilege
USER tester

RUN go install github.com/kadel/odo-robot@965ea0dd848856691bfc76e6824a8b787b950045 && \
    ibmcloud plugin install -f cloud-object-storage && \
    ibmcloud plugin install -f kubernetes-service
