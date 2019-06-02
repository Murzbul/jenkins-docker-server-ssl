# Jenkins Server with Docker, SSL and Nginx Proxy

## Configuration for production environment

1. Modified proxy/create.conf, proxy/default.conf, docker-compose.yml.
  1. Replace domain.web for a domain or subdomain of your choice.
2. Create SSL certificates to production environment
  1. Execute "docker-compose up" command with old.docker-compose.yml file. ( docker-compose up -f old.docker-compose.yml -d). This command execute a proxy version of http protocol in port 80 for to certbot
  find it an URL.
  2. Execute this command "sudo openssl dhparam -out services/prod/proxy/dh-param/dhparam-2048.pem 2048"
   This command generate a pem key to use in SSL proxy conf. (Use without a quotes)
  3. Execute this command.
  docker run -it --rm \
  -v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
  -v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
  -v /usr/share/nginx/html:/usr/share/nginx/html \
  -v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
  certbot/certbot \
  certonly --webroot \
  --email {your@email.com} --agree-tos --no-eff-email \
  --webroot-path=/usr/share/nginx/html \
  -d {domain.web}
  With this command executed certbot to generated a SSL certificates. Replace your email and domain.web like other configs.
  4. Execute "docker-compose down". (Maybe other docker compose with flags is required).
  With this command proxy server is down.
  5. Execute "docker-compose up" command.
  With command proxy server its working with SSL.
  6. To renew every 3 months. You can also add this command on your host server with a crontab so you can forget about it manually every 3 months.
  docker run --rm -v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
  -v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
  -v /usr/share/nginx/html:/usr/share/nginx/html \
  -v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
      certbot/certbot renew
  7. Restart proxy container docker-compose restart proxy   

## License

Code published under AFFERO GPL v3 (see [LICENSE-AGPLv3.txt](LICENSE-AGPLv3.txt))
