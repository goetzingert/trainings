# Deployment: Optiplex hinter IONOS-VPS

```
Internet ──► IONOS-VPS (TLS, Reverse-Proxy) ══WireGuard══► Optiplex :8088 ──► Container ki-websites (nginx)
                                                                              ├─ amtsklug.de
                                                                              ├─ kiklug.de
                                                                              └─ Weiterleitungen
```

Ein Container liefert beide Sites aus und unterscheidet sie am Hostnamen. Die Zusatzdomains leitet er weiter:

| Domain | Ziel |
|---|---|
| `amtsklug.de` | Site Amtsklug |
| `kiklug.de` | Site KI-klug |
| `www.*`, `ki-klug.de` | 301 auf die Hauptdomain |
| `kiregelwerk.de` | 301 auf `https://kiklug.de/ki-regelwerk.html` |
| `ki-in-der-verwaltung.de` | 301 auf `https://amtsklug.de/` |

## 1. DNS bei IONOS

Für jede Domain zwei A-Records auf die öffentliche IP des VPS: `@` und `www`. Falls der VPS IPv6 hat, zusätzlich AAAA-Records.

## 2. Optiplex: Container starten

Das Repository auf den Optiplex holen und den Container starten:

```bash
git clone https://github.com/goetzingert/trainings.git ~/trainings
```

```bash
cd ~/trainings/website/deploy && WG_IP=10.8.0.2 docker compose up -d
```

`10.8.0.2` durch die WireGuard-Adresse des Optiplex ersetzen. Test vom VPS aus:

```bash
curl -H "Host: amtsklug.de" http://10.8.0.2:8088/ | head -5
```

**Aktualisieren:** Die Dateien sind nur eingehängt, ein `git pull` genügt, kein Neustart nötig.

```bash
cd ~/trainings && git pull
```

Nur nach Änderungen an `nginx.conf` den Container neu laden:

```bash
docker exec ki-websites nginx -s reload
```

## 3. VPS: Reverse-Proxy und Zertifikate

**Mit nginx:** Die Datei `vps-nginx.conf.example` nach `/etc/nginx/sites-available/ki-websites` kopieren, die WireGuard-Adresse anpassen, aktivieren und Zertifikate holen:

```bash
sudo ln -s /etc/nginx/sites-available/ki-websites /etc/nginx/sites-enabled/ && sudo nginx -t && sudo systemctl reload nginx
```

```bash
sudo certbot --nginx -d amtsklug.de -d www.amtsklug.de -d kiklug.de -d www.kiklug.de -d ki-klug.de -d www.ki-klug.de -d kiregelwerk.de -d www.kiregelwerk.de -d ki-in-der-verwaltung.de -d www.ki-in-der-verwaltung.de
```

Certbot ergänzt den HTTPS-Block und die Weiterleitung von HTTP auf HTTPS selbst.

**Mit Caddy** (holt Zertifikate automatisch), im Caddyfile:

```
amtsklug.de, www.amtsklug.de, kiklug.de, www.kiklug.de, ki-klug.de, www.ki-klug.de, kiregelwerk.de, www.kiregelwerk.de, ki-in-der-verwaltung.de, www.ki-in-der-verwaltung.de {
    reverse_proxy 10.8.0.2:8088
}
```

**Mit Nginx Proxy Manager oder Traefik:** je Domain einen Proxy-Host auf `10.8.0.2:8088` anlegen; wichtig ist nur, dass der Host-Header unverändert weitergegeben wird (Standard bei beiden).

## Vor dem Livegang

- [ ] Platzhalter `[...]` in beiden Sites ersetzen (Name, E-Mail, Telefon)
- [ ] `impressum.html` und `datenschutz.html` in beiden Ordnern anlegen (Links im Footer zeigen schon darauf)
- [ ] Google Fonts lokal einbinden (Datenschutz)
- [ ] Nach dem Start alle Domains im Browser prüfen, auch `http://` und `www.`
