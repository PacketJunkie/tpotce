FROM alpine
  
# Include dist
ADD dist/ /root/dist/

# Install packages
RUN apk -U --no-cache add \ 
            		build-base \
            		git \
            		libcap \
            		libffi-dev \
            		libressl-dev \
                        libzmq \
            		postgresql-dev \
            		python3 \
            		python3-dev \
            		py-virtualenv && \
    pip3 install --no-cache-dir --upgrade pip && \

# Setup heralding
    mkdir -p /opt && \
    cd /opt/ && \
    git clone --depth=1 https://github.com/johnnykv/heralding && \
    cd heralding && \
    pip3 install --no-cache-dir -r requirements.txt && \
    pip3 install --no-cache-dir . && \

# Setup user, groups and configs
    addgroup -g 2000 heralding && \
    adduser -S -H -s /bin/ash -u 2000 -D -g 2000 heralding && \
    mkdir -p /var/log/heralding/ /etc/heralding && \
    mv /root/dist/heralding.yml /etc/heralding/ && \
    setcap cap_net_bind_service=+ep /usr/bin/python3.6 && \
    chown -R heralding:heralding /var/log/heralding && \

# Clean up
    apk del --purge \
            build-base \
            git \
            libcap \
            libffi-dev \
            libressl-dev \
            postgresql-dev \
            python3-dev \
            py-virtualenv && \
    rm -rf /root/* \
           /var/cache/apk/* \
           /opt/heralding

# Start elasticpot
STOPSIGNAL SIGINT
WORKDIR /tmp/heralding/
USER heralding:heralding
CMD exec heralding -c /etc/heralding/heralding.yml -l /var/log/heralding/heralding.log
