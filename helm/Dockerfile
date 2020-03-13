FROM gcr.io/cloud-builders/gcloud-slim

ARG HELM_VERSION=v2.16.1
ENV HELM_VERSION=$HELM_VERSION

ENV SOPS_VERSION=3.5.0
ENV SOPS_SHASUM=27369eb0a50fd7c34f4a01fa8bbfee617a5883a83ad1118c377a21caf1a1a655

COPY helm.bash /builder/helm.bash

RUN chmod +x /builder/helm.bash && \
  mkdir -p /builder/helm && \
  apt-get update && \
  apt-get install -y curl && \
  curl -SL https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -o helm.tar.gz && \
  tar zxvf helm.tar.gz --strip-components=1 -C /builder/helm linux-amd64 && \
  rm helm.tar.gz && \
  curl -Lo sops.deb https://github.com/mozilla/sops/releases/download/v${SOPS_VERSION}/sops_${SOPS_VERSION}_amd64.deb && \
  echo "$SOPS_SHASUM  sops.deb" | sha256sum --check && \
  dpkg -i sops.deb && \
  rm sops.deb && \
  apt-get --purge -y autoremove && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENV PATH=/builder/helm/:$PATH

ENTRYPOINT ["/builder/helm.bash"]
