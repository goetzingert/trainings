# Deployment: Optiplex mit Traefik, IONOS-VPS als Zugang

```
Internet ──► IONOS-VPS ══WireGuard══► Optiplex: Traefik (TLS, Let's Encrypt) ──► Container ki-websites (nginx)
                                                                                  ├─ amtsklug.de
                                                                                  ├─ kiklug.de
                                                                                  └─ Weiterleitungen
```

Traefik übernimmt Routing und Zertifikate. Der Container `ki-websites` braucht keinen veröffentlichten Port; er hängt im Docker-Netzwerk von Traefik und wird über Labels eingebunden. nginx im Container unterscheidet die Sites am Hostnamen und leitet die Zusatzdomains weiter:

| Domain | Ziel |
|---|---|
| `amtsklug.de` | Site Amtsklug |
| `kiklug.de` | Site KI-klug |
| `www.*`, `ki-klug.de` | 301 auf die Hauptdomain |
| `kiregelwerk.de` | 301 auf `https://kiklug.de/ki-regelwerk.html` |
| `ki-in-der-verwaltung.de` | 301 auf `https://amtsklug.de/` |

## 1. DNS bei IONOS

Für jede Domain zwei A-Records auf die öffentliche IP des VPS: `@` und `www`, genau wie bei Ihren bestehenden Sites.

## 2. VPS prüfen

Leitet der VPS die Ports 80 und 443 pauschal durch den Tunnel weiter (z. B. per iptables-DNAT oder nginx `stream` ohne Hostliste), ist nichts zu tun. Führt er eine Liste von Hostnamen (SNI-Map), die neuen Domains dort ergänzen.

## 3. Optiplex: Werte an Traefik anpassen

Die Namen von Netzwerk, Entrypoint und Zertifikats-Resolver aus Ihrer bestehenden Traefik-Konfiguration ablesen, zum Beispiel an den Labels einer laufenden Site:

```bash
docker inspect <bestehender-container> --format '{{json .Config.Labels}}'
```

```bash
docker network ls
```

Weichen sie von den Standardwerten (`proxy`, `websecure`, `letsencrypt`) ab, eine Datei `website/deploy/.env` anlegen:

```
TRAEFIK_NETWORK=traefik_default
ENTRYPOINT=https
CERTRESOLVER=le
```

## 4. Container starten

```bash
git clone https://github.com/goetzingert/trainings.git ~/trainings
```

```bash
cd ~/trainings/website/deploy && docker compose up -d
```

Konfiguration im Container prüfen:

```bash
docker exec ki-websites nginx -t
```

Traefik holt die Zertifikate beim ersten Aufruf jeder Domain; das kann einige Sekunden dauern. Im Traefik-Log sieht man, ob es geklappt hat:

```bash
docker logs <traefik-container> 2>&1 | grep -i -E "acme|amtsklug|kiklug"
```

**HTTP auf HTTPS:** Die Labels binden nur den HTTPS-Entrypoint. Leitet Ihr Traefik `web` (Port 80) bereits global auf HTTPS um, ist das abgedeckt; sonst wie bei den bestehenden Sites einen Redirect-Router ergänzen.

## Aktualisieren

Die Dateien sind nur eingehängt, ein `git pull` genügt, kein Neustart nötig:

```bash
cd ~/trainings && git pull
```

Nur nach Änderungen an `nginx.conf`:

```bash
docker exec ki-websites nginx -s reload
```

## Vor dem Livegang

- [ ] Platzhalter `[...]` in beiden Sites ersetzen (Name, E-Mail, Telefon)
- [ ] `impressum.html` und `datenschutz.html` in beiden Ordnern anlegen (Links im Footer zeigen schon darauf)
- [ ] Google Fonts lokal einbinden (Datenschutz)
- [ ] Nach dem Start alle Domains im Browser prüfen, auch `http://` und `www.`
