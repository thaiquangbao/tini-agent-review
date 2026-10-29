# Tini in a container: the dashboard (port 9000) plus any gateway whose token
# is set (Telegram / Discord / WhatsApp start automatically with it).
#
#   docker build -f docker/Dockerfile -t tini-agent .
#   docker run --env-file .env -p 127.0.0.1:9000:9000 -v tini-data:/data tini-agent
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    TINI_HOME=/data \
    TINI_DASHBOARD_HOST=0.0.0.0 \
    TINI_DASHBOARD_PORT=9000

WORKDIR /app

# Only what the wheel build needs: pyproject.toml reads README.md, and the
# wheel force-includes skills/.
COPY pyproject.toml README.md ./
COPY skills ./skills
COPY tini ./tini
RUN pip install ".[telegram,discord,whatsapp]"

# Memory, calendar, chat log, traces and SOUL.md live here — mount a volume.
VOLUME ["/data"]
EXPOSE 9000

CMD ["tini", "dashboard"]
