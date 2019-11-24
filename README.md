# DOCKER - PHP 7.3 + Apache + Wordpress

Tecnologias

* PHP 7.3
* Apache 2
* Wordpress

## Instalação

Clone este repositório, entre no diretorio e  execute `docker-compose up -d`.

```
git clone do projeto e executar docker-compose up -d
```


Configure o NGINX, no diretório `/etc/nginx/conf.d/`, crie um arquivo `site-edebe.conf` com o conteúdo:

```
# /var/docker/site_edebe
    server {
        listen 80;

        proxy_connect_timeout       600;
        proxy_send_timeout          600;
        proxy_read_timeout          600;
        send_timeout                600;
        server_name wwwelb.edebe.com.br sitev2.edebe.com.br www.edebe.com.br edebe.com.br www2.edebe.com.br www312.edebe.com.br;

        #rewrite ^/(.*)$ http://www.edebe.com.br/$1 permanent;
        location / {
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   Host      $http_host;
            proxy_set_header   Host      www.edebe.com.br;
            proxy_pass         http://127.0.0.1:8011;
        }
    }


# /var/docker/site_edebe
    server {
        listen 443 ssl;

        ssl_certificate edebe.nokey.pem;
        ssl_certificate_key kedebe.key;

        proxy_connect_timeout       600;
        proxy_send_timeout          600;
        proxy_read_timeout          600;
        send_timeout                600;
        server_name wwwelb.edebe.com.br sitev2.edebe.com.br www.edebe.com.br edebe.com.br www2.edebe.com.br www312.edebe.com.br;
        
        #rewrite ^/(.*)$ http://www.edebe.com.br/$1 permanent;
        
        location / {
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   Host      $http_host;
            proxy_set_header   Host      www.edebe.com.br;
            proxy_pass         http://127.0.0.1:8011;
        }
    }

```


Reinicie o Nginx: `systemctl restart nginx.service`

## Verificar logs do Servidor NGINX!

```
tail -f access_log /var/log/nginx/site-edebe.access.log
tail -f error_log /var/log/nginx/site-edebe.error.log
```
## Verificar logs do Container APACHE!
```
tail -f /var/docker/site-edebe/logs/apache2/site.edebe.com.br-access_log
tail -f /var/docker/site-edebe/logs/apache2/site.edebe.com.br-error_log
```
