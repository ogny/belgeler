
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/orkun.design/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/orkun.design/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot



}
server {
    if ($host = www.orkun.design) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = orkun.design) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        listen 80 ;
        listen [::]:80 ;
    server_name www.orkun.design orkun.design;
    return 404; # managed by Certbot

}

* haproxy icin 

certbot certonly            \
    --standalone            \
    -d <domain_name >       \
    --non-interactive       \
    --agree-tos             \
    --email <e-mail>        \
    --http-01-port=8888

docker run                                                  \
    --detach                                                \
    --name grafana                                          \
    --env "VIRTUAL_HOST=othersubdomain.yourdomain.tld"      \
    --env "VIRTUAL_PORT=3000"                               \
    --env "LETSENCRYPT_HOST=othersubdomain.yourdomain.tld"  \
    --env "LETSENCRYPT_EMAIL=mail@yourdomain.tld"           \
    grafana/grafana

