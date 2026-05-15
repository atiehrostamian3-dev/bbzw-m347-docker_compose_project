# Dockerfile für Multi-Architektur-Build
# Kompatibel mit: linux/amd64, linux/arm64, linux/arm/v7 (Raspberry Pi)

# --- Builder Stage ---
#
# Option A (diese Lösung): python:3.12-slim + build-essential
#   → Basisimage: ~130 MB, build-essential: ~200 MB → Builder-Layer total ~330 MB
#   → Nur das Nötigste wird installiert; schnellerer Download & Entpacken unter QEMU
#
# Option B (langsamer): FROM python:3.12
#   → Basisimage: ~1.7 GB – alles inklusive, aber riesiger Download unter QEMU-Emulation
#   → Für ARM-Builds oft 3–5x langsamer, weil jedes MB emuliert entpackt werden muss
#
# Fazit: slim + build-essential ist der bessere Kompromiss für Cross-Plattform-Builds.

FROM python:3.12-slim AS builder
RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY requirements.txt .
RUN pip install --user -r requirements.txt

# ---------------------------------------------------------------------------
# OPTIONAL – Experiment: BUILDPLATFORM vs. TARGETPLATFORM (Schritt 6)
#
# Ersetze die FROM-Zeile oben durch:
#   FROM --platform=$BUILDPLATFORM python:3.12-slim AS builder
#
# Was passiert:
#   → Builder läuft NATIV auf dem Host (z.B. amd64) – kein QEMU → viel schneller
#   → ABER: C-Extensions (ujson, uvloop, httptools) werden für amd64 kompiliert,
#     nicht für arm/v7 → der Container crasht beim Start auf dem Raspberry Pi!
#
# Zum Beobachten der Zielplattform folgende Zeilen nach FROM einfügen:
#   ARG TARGETARCH
#   ARG TARGETPLATFORM
#   RUN echo "Zielplattform: $TARGETPLATFORM, Architektur: $TARGETARCH"
#
# Fazit: --platform=$BUILDPLATFORM funktioniert nur für reine Python-Packages
# oder wenn Wheels gezielt für die Zielarchitektur heruntergeladen werden.
# ---------------------------------------------------------------------------

# --- Runtime Stage ---
# Bleibt in beiden Optionen python:3.12-slim – der Builder wird weggeworfen,
# nur /root/.local (installierte Pakete) wird übernommen.
FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY src/ .
ENV PATH=/root/.local/bin:$PATH
ENV PYTHONUNBUFFERED=1
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
