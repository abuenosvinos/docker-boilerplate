# Versions to use.
ARG ALPINE_VERSION=3.17.3
ARG NGINX_VERSION=alpine

# Prepare an Alpine-based image with OpenSSL.
FROM alpine:${ALPINE_VERSION} as alpine
ARG ORG_NAME=myapp
ARG DOMAIN_NAME=*.myapp
ARG DAYS_VALID=365

# https://betterprogramming.pub/nginx-self-signed-certificate-docker-f3861c0f03e

RUN apk add --no-cache openssl
RUN echo "Creating self-signed certificate valid for ${DAYS_VALID} days for domain ${DOMAIN_NAME}" && \
    openssl \
    req -x509 \
    -nodes \
    -subj "/CN=${ORG_NAME}/C=ES/ST=Spain/L=Barcelona/O=${ORG_NAME}/OU=Development" \
    -addext "subjectAltName=DNS:${DOMAIN_NAME}" \
    -days ${DAYS_VALID} \
    -newkey rsa:2048 \
    -keyout /tmp/myapp.key \
    -out /tmp/myapp.crt

FROM nginx:${NGINX_VERSION} as nginx

COPY --from=alpine /tmp/myapp.key /etc/ssl/private
COPY --from=alpine /tmp/myapp.crt /etc/ssl/certs

CMD ["nginx"]

EXPOSE 80 443

WORKDIR /var/www