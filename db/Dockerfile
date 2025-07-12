FROM ubuntu:22.04

ARG FDB_VERSION=7.1.42
ARG DEB_REVISION=1

RUN apt-get update && apt-get install -y curl gnupg lsb-release

# Download and install FoundationDB server & client
RUN curl -sSL https://www.foundationdb.org/downloads/${FDB_VERSION}/ubuntu/installers/foundationdb-clients_${FDB_VERSION}-${DEB_REVISION}_amd64.deb -o client.deb \
 && curl -sSL https://www.foundationdb.org/downloads/${FDB_VERSION}/ubuntu/installers/foundationdb-server_${FDB_VERSION}-${DEB_REVISION}_amd64.deb -o server.deb \
 && dpkg -i client.deb server.deb \
 && rm client.deb server.deb

# Clean any old data, ensure binaries exist
RUN rm -rf /var/lib/foundationdb/data/*

ENTRYPOINT ["/usr/bin/fdbmonitor"]

