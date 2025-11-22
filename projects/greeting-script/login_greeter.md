#!/usr/bin/env bash
CITY="Kansas City"
STATE="MO"

RESET="\e[0m"
DIM="\e[2m"

# Colors (red, green, yellow, blue, magenta, cyan) – pick one at random for the header
COLORS=("\e[1;31m" "\e[1;32m" "\e[1;33m" "\e[1;34m" "\e[1;35m" "\e[1;36m")
BOLD=${COLORS[$RANDOM % ${#COLORS[@]}]}

# simple print helper (avoids echo -e quirks)
say() { printf "%b\n" "$1"; }

greet() {
  local now
  now=$(date "+%A, %B %d, %Y | %I:%M %p")
  say "\n${BOLD}Welcome back, $(whoami)!${RESET}"
  say "${DIM}$now${RESET}\n"
}

weather() {
  local enc_city="${CITY// /%20}"
  say "${DIM}Weather:${RESET}"
  curl -s "https://wttr.in/${enc_city}?format=3"
  say ""
}

# ====================================
# LOCAL NEWS SECTION
# ====================================
local_news() {
  say ""
  say "${BOLD}📰 Kansas City Headlines:${RESET}"

  python3 - <<'PY'
import html, xml.etree.ElementTree as ET, urllib.request, ssl, sys
ssl._create_default_https_context = ssl._create_unverified_context

# Try multiple feeds; stop after we print 5 headlines
feeds = [
  # Google News – KC local
  "https://news.google.com/rss/search?q=Kansas%20City%20Missouri%20local&hl=en-US&gl=US&ceid=US:en",
  # KCUR (NPR Kansas City)
  "https://www.kcur.org/rss.xml",
  # Backup Google News query
  "https://news.google.com/rss/search?q=Kansas%20City&hl=en-US&gl=US&ceid=US:en",
]

def osc8(url, text):
    # Clickable link in VS Code, GNOME Terminal, iTerm2
    return f"\033]8;;{url}\007{text}\033]8;;\007"

printed = 0

for url in feeds:
    try:
        with urllib.request.urlopen(url, timeout=6) as r:
            data = r.read()
        root = ET.fromstring(data)
        for it in root.findall(".//item"):
            title = (it.findtext("title") or "").strip()
            link  = (it.findtext("link") or "").strip()
            if not title:
                continue
            title = html.unescape(title)
            if link:
                print(" • " + osc8(link, title))
            else:
                print(" • " + title)
            printed += 1
            if printed >= 5:
                raise SystemExit(0)
    except Exception:
        # Try next feed
        continue

if printed == 0:
    print(" • (No KC headlines available)")
PY

  say ""
}

dad_joke() {
  say "${DIM}Dad Joke:${RESET}"
  curl -s https://icanhazdadjoke.com/ -H "Accept: text/plain"
  say ""
}

quote() {
  say "${DIM}Quote of the Day:${RESET}"
  curl -s https://zenquotes.io/api/random | grep -oP '"q":"\K[^"]+' | head -n 1
  say ""
}

greet
weather
local_news
dad_joke
quote
say ""
