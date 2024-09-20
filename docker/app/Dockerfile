# ベースイメージとして nginx 1.27.1 を使用
FROM nginx:1.27.1

# 環境変数を設定
ENV DEBIAN_FRONTEND noninteractive

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    lsb-release \
    ca-certificates \
    apt-transport-https \
    software-properties-common \
    wget \
    gnupg2 \
    && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list \
    && apt-get update \
    && apt-get install -y \
    php8.3-fpm \
    php8.3-mysql \
    php8.3-common \
    php8.3-curl \
    php8.3-mbstring \
    php8.3-xml \
    php8.3-zip \
    msmtp-mta \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# メール設定
COPY ./msmtprc /etc/msmtprc

# アプリケーションディレクトリの設定
WORKDIR /var/www/html

# PHP-FPM の設定
RUN sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php/8.3/fpm/php.ini
RUN sed -i 's/listen = \/run\/php\/php8.3-fpm.sock/listen = 9000/' /etc/php/8.3/fpm/pool.d/www.conf

# Nginx の設定
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# PHP-FPM を起動
CMD service php8.3-fpm start && nginx -g 'daemon off;'