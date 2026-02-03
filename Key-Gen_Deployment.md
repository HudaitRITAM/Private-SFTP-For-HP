# Key-Gen Service Deployment Guide

---

## Clone the Repository

```bash
cd ~
git clone https://gitlab.nest.gov.tt/root/private-sftp.git
```

---

## Move and Unzip key-gen

```bash
cd ~/private-sftp
mv key-gen.zip /home/eidadmin/
cd /home/eidadmin
unzip key-gen.zip
```

Result:

```text
/home/eidadmin/key-gen
```

---

## Folder Structure

```text
key-gen/
├── keygen-new-version.py
├── key.sh
├── redis_client.py
├── venv/
├── nohup.out
└── README.md
```

---

## Update API URL according to your apisix url

Edit the file:

```bash
cd ~/key-gen
nano keygen-new-version.py
```

Change:

```python
API_URL = "http://10.0.0.5:9080/apk/creds"
```

Save and exit.

---

## Install System Dependencies (One Time)

```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv net-tools curl
```

---

## Create Python Virtual Environment

```bash
cd ~/key-gen
python3 -m venv venv
source venv/bin/activate
```

---

## Install Python Libraries

```bash
pip install --upgrade pip
pip install fastapi uvicorn requests pydantic
```

---

## Configure Environment Variables

Load variables:

```bash
source key.sh
```

---

## Start the Service

```bash
cd ~/key-gen
source venv/bin/activate
source key.sh

nohup venv/bin/python -m uvicorn keygen-new-version:app \
  --host 0.0.0.0 \
  --port 7000 \
  > nohup.out 2>&1 &
```

---
