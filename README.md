---
title: "🧰 ETL desde Google Sheets — Consolidación 2023–2025"
description: >
  Este repositorio contiene un Jupyter Notebook que implementa un flujo de extracción
  y transformación de datos (ET/ETL) desde Google Sheets y consolida información
  de múltiples hojas/años (2023, 2024, 2025) en un único DataFrame listo para análisis.

summary: |
  Se autentica con una cuenta de servicio (credenciales.json), lee datos de Google Sheets,
  normaliza columnas y estandariza textos, fechas y países. Produce un dataset consolidado
  y puede incluir una etapa opcional de carga (Load) a CSV, BD o nueva hoja de Google Sheets.

diagram: |
  Google Sheets (múltiples hojas / años)
            ↓
        Extract (gspread)
            ↓
      Transform (pandas: limpieza de columnas, normalización de texto, parseo de fechas, estandarización de países)
            ↓
     DataFrame consolidado ✅
            ↓
     (opcional) Load: CSV / BD / otro Sheet

features:
  - "🔐 Autenticación vía Service Account (credenciales.json)"
  - "📥 Lectura desde Google Sheets usando gspread y gspread_dataframe"
  - "🧹 Normalización de columnas y nombres"
  - "🗓️ Parseo de fechas heterogéneas"
  - "🌎 Estandarización de países"
  - "🔤 Limpieza de texto (acentos y caracteres no deseados)"
  - "🧱 Consolidación de información 2023–2025"
  - "⏳ Reintentos simples con time.sleep para evitar límites de API"

dependencies:
  - pandas
  - numpy
  - gspread
  - oauth2client
  - gspread-dataframe
  - built-in: [time, re, unicodedata, datetime]

credentials: |
  1. Crea una Service Account en Google Cloud Console y descarga el JSON.
  2. Coloca el archivo en el proyecto como credenciales.json.
  3. Comparte tus Google Sheets con el correo de la service account (lector/editor).
  4. Scopes usados:
     - https://www.googleapis.com/auth/spreadsheets
     - https://www.googleapis.com/auth/drive
  ⚠️ Nunca subas credenciales.json a GitHub. Añádelo a .gitignore.

usage: |
  1. Abre ETL_google_sheets.ipynb en Jupyter o VS Code.
  2. Asegúrate de tener credenciales.json en el mismo directorio.
  3. Ejecuta todas las celdas.
  4. Obtendrás un DataFrame consolidado con los datos 2023–2025.

functions:
  - name: normalizar_columna
    description: "Homogeniza nombres de columnas (minúsculas, sin acentos, sin espacios raros)."
  - name: parse_fecha_mixta_especifica
    description: "Convierte fechas con formatos mixtos a un datetime consistente."
  - name: _norm_txt
    description: "Limpia texto eliminando acentos y normalizando caracteres."
  - name: normalize_country
    description: "Estandariza nombres de países."

process:
  - "Importa librerías: pandas, numpy, gspread, oauth2client.service_account, gspread_dataframe"
  - "Autenticación con ServiceAccountCredentials.from_json_keyfile_name('credenciales.json')"
  - "Lee múltiples hojas de Google Sheets por año (2023, 2024, 2025)"
  - "Normaliza columnas, fechas y países"
  - "Concatena todos los datos en un único DataFrame"

etl_stage: "Extract + Transform (puede ampliarse a ETL añadiendo carga final)"

load_examples:
  - csv: "df_final.to_csv('consolidado_2023_2025.csv', index=False)"
  - google_sheets: |
      from gspread_dataframe import set_with_dataframe
      ws_out = sh.worksheet("consolidado")
      set_with_dataframe(ws_out, df_final)
  - database: |
      from sqlalchemy import create_engine
      engine = create_engine("postgresql+psycopg2://user:pass@host:5432/db")
      df_final.to_sql("consolidado_gsheets", engine, if_exists="replace", index=False)

validations:
  - "Verifica tipos de datos con df.dtypes"
  - "Revisa duplicados y nulos clave"
  - "Controla outliers numéricos"
  - "Valida formatos de fecha consistentes"

best_practices:
  - "Usar .gitignore para credenciales y datos sensibles"
  - "Limpiar salidas del notebook antes de hacer commit"
  - "Dividir funciones largas en módulos (utils.py)"
  - "Agregar pruebas básicas (asserts sobre conteos/fechas)"

repo_structure: |
  .
  ├── ETL_google_sheets.ipynb     # Notebook principal
  ├── requirements.txt            # Dependencias (opcional)
  ├── README.md                   # Este archivo
  ├── data/                       # CSVs o salidas locales (opcional)
  └── credenciales.json           # No subir a git

troubleshooting:
  - "AuthError / 403: comparte el Sheet con la Service Account"
  - "Rate limit / timeouts: usa time.sleep o lotes más pequeños"
  - "Fechas incorrectas: revisa parse_fecha_mixta_especifica"

license: "MIT (o la que prefieras)"
note: >
  Si lo deseas, se puede generar automáticamente un diccionario de datos
  con nombres de columnas, tipos y ejemplos detectados desde el notebook.
---
