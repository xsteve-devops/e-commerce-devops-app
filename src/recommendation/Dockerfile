FROM python:3.12-slim

WORKDIR /usr/src/app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV RECOMMENDATION_PORT=8080

RUN useradd -r -u 10001 appuser
USER appuser

ENTRYPOINT ["python", "recommendation_server.py"]  