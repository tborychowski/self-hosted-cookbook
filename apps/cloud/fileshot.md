# FileShot

- Zero-knowledge, client-side AES-256-GCM encryption before upload.
- Server stores only ciphertext — decryption keys never leave the browser.
- No account required for sender or recipient.
- Optional password protection and expiration settings.

<br>

- [Homepage](https://fileshot.io)
- [GitHub repo](https://github.com/FileShot/FileShotZKE)


## docker-compose.yml
```yaml
services:
  fileshot:
    image: nginx:alpine
    container_name: fileshot
    restart: unless-stopped
    ports:
      - "3000:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
```

Clone the static files into an `html` folder in the same directory as your `docker-compose.yml` before starting:
```sh
git clone https://github.com/FileShot/FileShotZKE html
```

App will be available at: `<server IP>:3000`
