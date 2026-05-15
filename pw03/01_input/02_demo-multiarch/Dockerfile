# syntax=docker/dockerfile:1

# Stage 1 – Build
# Läuft unter QEMU-Emulation auf der ZIEL-Plattform (z. B. linux/arm/v7)
# build-essential liefert gcc/g++ für Packages ohne vorgefertigte ARM-Wheels
FROM python:3.12-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*

WORKDIR /build
COPY requirements.txt .
RUN pip install --user -r requirements.txt

# Stage 2 – Runtime
# Dieses Stage läuft auf der ZIEL-Plattform (z. B. linux/arm64)
FROM python:3.12-slim

WORKDIR /app

# Installierte Pakete aus dem Builder übernehmen
COPY --from=builder /root/.local /root/.local

# App-Code kopieren
COPY src/ .

# Plattform-Info beim Start ausgeben (zeigt dass ARM wirklich läuft)
ENV PATH=/root/.local/bin:$PATH
ENV PYTHONUNBUFFERED=1

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
