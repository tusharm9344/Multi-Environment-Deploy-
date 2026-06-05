FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN apt-get update && apt-get install -y curl
RUN pip install --no-cache-dir -r  requirements.txt
COPY . .
EXPOSE 5000
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=10s \
	CMD curl -f http://localhost:5000/ || exit 1
CMD ["python","app.py"]
