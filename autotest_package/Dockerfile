FROM python:3.12-slim

ENV DEBIAN_FRONTEND=noninteractive

# Install gnupg + wget first, then add Chrome repo with modern gpg approach
# (apt-key is removed in Debian 13 trixie)
RUN apt-get update && apt-get install -y \
    wget gnupg ca-certificates \
    --no-install-recommends \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub \
       | gpg --dearmor -o /usr/share/keyrings/google-chrome.gpg \
    && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
       > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get install -y google-chrome-stable --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app/autotest_package

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt uvicorn

COPY . .

RUN mkdir -p /app/autotest_output

EXPOSE 8000

CMD ["uvicorn", "api:app", "--host", "0.0.0.0", "--port", "8000"]
