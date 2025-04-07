FROM dunglas/frankenphp:php8.2

WORKDIR /opt
RUN groupadd -g 1000 appuser && usermod -u 1000 appuser
RUN curl -sL $(curl -s https://api.github.com/repos/mattn/goreman/releases/latest | awk '/browser_download_url.*linux_amd64/ {print $2}' | sed -r 's/\"//g') -o goreman.tar.gz && tar zxf goreman.tar.gz && find . -type f -name goreman -exec cp {} /opt/ \; && rm -rf goreman*
RUN apt update && \
    apt upgrade -y && \
    apt install libfreetype6 msmtp cronie -y && \
    apt clean && \
    install-php-extensions mysqli gd intl zip opcache memcache
COPY Caddyfile /etc/caddy/Caddyfile
COPY Procfile /app/
USER appuser
WORKDIR /app
EXPOSE 80, 443, 443/udp
CMD ["goreman", "start"]