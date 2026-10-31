# Deployment: Optiplex mit Traefik (File-Provider), IONOS-VPS als Zugang

```
Internet ──► IONOS-VPS ══WireGuard══► Optiplex: Traefik (TLS, le-ionos) ──► 127.0.0.1:8090 ──► Container ki-websites (nginx)
                                                                                                ├─ amtsklug.de
                                                                                                ├─ kiklug.de
                                                                                                └─ Weiterleitungen
```

Wie die bestehenden Sites: Traefik bekommt **keinen** Docker-Socket, sondern eine eigene YAML-Datei im File-Provider. Der Container veröffentlicht seinen Port nur auf `127.0.0.1`.

| Datei | Zweck |
|---|---|
| `docker-compose.yml` | nginx-Container, Port `127.0.0.1:8090` |
| `nginx.conf` | Unterscheidet die Sites am Hostnamen, leitet Zusatzdomains weiter |
| `traefik-ki-websites.yml` | Router und Service für Traefik (File-Provider) |

| Domain | Ziel |
|---|---|
| `amtsklug.de` | Site Amtsklug |
| `kiklug.de` | Site KI-klug |
| `www.*`, `ki-klug.de` | 301 auf die Hauptdomain |
| `kiregelwerk.de` | 301 auf `https://kiklug.de/ki-regelwerk.html` |
| `ki-in-der-verwaltung.de` | 301 auf `https://amtsklug.de/` |

## 1. DNS bei IONOS

Für jede Domain zwei A-Records auf die öffentliche IP des VPS: `@` und `www`, wie bei den bestehenden Sites. Der Resolver `le-ionos` nutzt vermutlich die DNS-Challenge über die IONOS-API; dafür müssen alle Domains im selben IONOS-Konto liegen, auf das der API-Schlüssel zugreift.

## 2. Container starten

```bash
git clone https://github.com/goetzingert/trainings.git ~/trainings
```

```bash
cd ~/trainings/website/deploy && docker compose up -d
```

```bash
docker exec ki-websites nginx -t
```

```bash
curl -s -H "Host: kiklug.de" http://127.0.0.1:8090/ | grep -o "<title>.*</title>"
```

Ist Port 8090 schon belegt, in `docker-compose.yml` und `traefik-ki-websites.yml` denselben anderen Port eintragen.

## 3. Traefik-Datei einbinden

`traefik-ki-websites.yml` in das Verzeichnis kopieren, das Ihr File-Provider überwacht (dort, wo auch die YAML der Coaching-Site liegt). Mit `watch: true` lädt Traefik sie ohne Neustart. Danach im Log prüfen, ob die Zertifikate geholt wurden:

```bash
docker logs <traefik-container> 2>&1 | grep -i -E "acme|amtsklug|kiklug"
```

Entrypoint (`websecure`) und Resolver (`le-ionos`) sind wie in Ihrer bestehenden Datei gesetzt.

## Aktualisieren

Die Site-Dateien sind nur eingehängt, ein `git pull` genügt, kein Neustart nötig:

```bash
cd ~/trainings && git pull
```

Nur nach Änderungen an `nginx.conf`:

```bash
docker exec ki-websites nginx -s reload
```

## Vor dem Livegang

- [x] Schriften lokal eingebunden (`fonts/` in beiden Sites)
- [ ] Platzhalter `[...]` in beiden Sites ersetzen, auch in `impressum.html` und `datenschutz.html`
- [ ] Impressum und Datenschutzerklärung prüfen (Vorlagen, keine Rechtsberatung)
- [ ] Nach dem Start alle Domains im Browser prüfen, auch `http://` und `www.`
