# Hybird Application System - Run Guide

This file contains the exact steps to run the app locally on Windows.

## 1) Open project folder

Open terminal and run:

```powershell
cd C:\TYY\typro
```

## 2) Activate Python virtual environment

If your environment is at `C:\TYY\.venv`:

```powershell
C:\TYY\.venv\Scripts\activate
```

If your environment is inside project as `venv`:

```powershell
c
```

## 3) Install dependencies (first time only)

```powershell
pip install -r requirements.txt
```

## 4) Run the Streamlit app

```powershell
streamlit run app.py
```

Alternative (explicit Python path):

```powershell
C:\TYY\.venv\Scripts\python.exe -m streamlit run app.py
```

## 5) Open in browser

Use:

- http://localhost:8501

## 6) If port 8501 is busy

```powershell
streamlit run app.py --server.port 8502
```

Then open:

- http://localhost:8502

## 7) Stop the app

In terminal where Streamlit is running, press:

- `Ctrl + C`

## Quick health check (optional)

```powershell
try { (Invoke-WebRequest -Uri http://localhost:8501 -UseBasicParsing -TimeoutSec 10).StatusCode } catch { $_.Exception.Message }
```

Expected output when running correctly:

- `200`
