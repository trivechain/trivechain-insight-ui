FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy trivechaincore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/trivechain/trivechaincore-node.git .

# Copy config file
COPY trivechaincore-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/trivechaincore-node install trivechain/trivechain-insight-api@${MAJOR_VERSION}
RUN bin/trivechaincore-node install trivechain/trivechain-insight-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="Trivechain Developers <dev@dash.org>"
LABEL description="Dockerised Insight-Trivechain"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/trivechaincore-node", "start"]
