
# iconsite-stack

<img width="1860" height="1080" alt="image" src="https://github.com/user-attachments/assets/be8a532a-e468-4459-86bd-163a0b144acd" />


Minimal reproducible Tor hidden service + Nginx static site stack.

This project runs:

* Nginx (serves static files)
* Tor (provides:

  * hidden service
  * optional SOCKS5 proxy)

Everything runs in Docker.
No host nginx. No host tor. No `/var/www`.

---

## Requirements

* Docker
* Docker Compose (v2)

---

## Run

```bash
docker compose up -d --build
```

Wait until Tor finishes bootstrapping:

```bash
docker compose logs -f tor
```

You should see:

```
Bootstrapped 100% (done)
```

---

## Get Onion Address

```bash
docker compose exec tor cat /var/lib/tor/hidden_service/hostname
```

Open the printed `.onion` address in **Tor Browser**.

---

## Use as SOCKS5 Proxy (optional)

The Tor container exposes a local SOCKS proxy:

```
127.0.0.1:9050
```

Test:

```bash
curl --socks5-hostname 127.0.0.1:9050 https://api.ipify.org
```

Browser settings:

* SOCKS Host: 127.0.0.1
* Port: 9050
* SOCKS v5
* Enable "Proxy DNS over SOCKS"

The proxy is bound to `127.0.0.1` only and is not exposed publicly.

---

## Project Structure

```
.
├── compose.yaml
├── nginx/
│   └── Dockerfile
├── tor/
│   ├── Dockerfile
│   └── torrc
└── site/
    └── public/
```

* `site/public/` — Hugo static output
* `nginx/` — serves static files
* `tor/` — runs Tor and creates hidden service

---

## Clean Stop

```bash
docker compose down
```

Remove volumes (including Tor keys):

```bash
docker compose down -v
```

Note: Removing volumes will generate a new onion address on next start.

---

## Notes

* No system Tor is required.
* No system Nginx is required.
* No host configuration changes.
* Each deployment generates its own onion address.

---

That’s it.
