FROM python:3.12 AS builder
WORKDIR /build
COPY requirements.txt .
RUN pip install --user -r requirements.txt 

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY src/ .
ENV PATH=/root/.local/bin:$PATH
CMD ["python", "main.py"] 