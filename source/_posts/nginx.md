#worker_processes 4;

events {
    worker_connections  1024;
}

error_log logs/error.log debug;


http {
    include       mime.types;
    default_type  application/octet-stream;
    proxy_buffering on;
    proxy_buffers 8 4k;
    proxy_buffer_size 4k;
    proxy_temp_path /var/nginx/temp;

    proxy_cache_path /var/nginx/cache/one  levels=1:2   keys_zone=one:10m max_size=2g  inactive=1d;
    proxy_cache_key "$host$request_uri";
    proxy_cache_valid  24h;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    upstream www.google.com {
        server 216.58.219.36:443;
        server 216.58.219.35:443;
        server 216.58.219.37:443;
    }

    server {
            listen 80;
            server_name g.jingyingying.com;
            return 301 https://g.jingyingying.com$request_uri;
    }

    server {
            proxy_cache one;

            server_name g.jingyingying.com;
            listen 443;

            ssl on;
            ssl_certificate ssl/1_g.jingyingying.com_bundle.crt;
            ssl_certificate_key ssl/ssl.key;
            ssl_prefer_server_ciphers on;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
            ssl_session_cache shared:SSL:10m;
            #ssl_ciphers EECDH+aRSA+AES;

            resolver 108.61.10.10;
            location / {
                    google on;
                    google_scholar on;
                    expires 10m;

            }
    }
}
                                                                                                                                        1,0-1         Top

