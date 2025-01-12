# Docmost Cooking Book

## Building

Download the docker compose file and change those variables to your needs:

```
environment:
  APP_URL: 'your-host-url'
  APP_SECRET: 'your-app-secret'
  DATABASE_URL: 'postgresql://docmost:your-db-password@db:5432/docmost?schema=public'
  REDIS_URL: 'redis://redis:6379'
  MAIL_DRIVER: 'smtp'
  MAIL_FROM_NAME: "Docmost"
  SMTP_HOST: "your-smtp-host"
  SMTP_PORT: 587
  SMTP_USERNAME: "your-smtp-username"
  SMTP_PASSWORD: "your-smtp-password"
  SMTP_FROM_EMAIL: "your-smtp-from-email"

db:
  image: postgres:16-alpine
  environment:
    POSTGRES_DB: docmost
    POSTGRES_USER: docmost
    POSTGRES_PASSWORD: your-db-password
  restart: unless-stopped
  volumes:
    - db_data:/var/lib/postgresql/data
```

Build the docker container:

```bash
docker compose -f docmost-compose.yml up -d
```

## Setup HTTPS 
To enable HTTPS you can use a reverse proxy like Caddy.

### Caddyfile

```
your.domain.com {

        # to use existing ssl certs
        # tls /data/ssl/your_fullchain.pem /data/ssl/your_privkey.pem

        # or to use internal certs made by Caddy
        tls internal
        
        reverse_proxy destination_host:destination_port {
                header_up X-Real-IP {http.request.remote}
                transport http {
                        read_timeout 3m
                        write_timeout 3m
                }
        }
}
```
