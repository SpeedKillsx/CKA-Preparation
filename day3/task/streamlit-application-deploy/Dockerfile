FROM python:3.13 AS installer

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PIP_NO_CACHE_DIR=1
ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /app

COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt
COPY . .
FROM python:3.13  AS deployer

WORKDIR /app

COPY --from=installer /usr/local /usr/local

COPY --from=installer /app /app


EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.enableCORS=false"]
