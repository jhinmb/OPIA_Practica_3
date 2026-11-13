# Aplicación Streamlit para el precio de casas en California

Esta aplicación permite estimar el precio de viviendas en California mediante inteligencia artificial. Está formada por una API desarrollada con FastAPI y una interfaz web con Streamlit. El usuario introduce el ingreso medio y el número de habitaciones. La interfaz envía los datos a la API mediante una petición POST y muestra el precio estimado en dólares.

**Autores:** Alexander Selivanov

## Servicios desplegados en Render

| Servicio | Qué es | URL |
|---|---|---|
| API | FastAPI (`app.py`) | [https://opia-practica-3.onrender.com](https://opia-practica-3.onrender.com) |
| Interfaz | Streamlit (`streamlit_app.py`) | [https://opia-practica-3-streamlit.onrender.com](https://opia-practica-3-streamlit.onrender.com) |

## Cómo ejecutarla en local

Instalar dependencias:

```bash
pip install -r requirements.txt
```

Terminal 1, iniciar la API:

```bash
uvicorn app:app --reload
```

Terminal 2, iniciar Streamlit:

```bash
streamlit run streamlit_app.py
```

La interfaz estará en `http://localhost:8501` y la API en `http://localhost:8000`. En la barra lateral de Streamlit hay que introducir la URL de la API.

## Despliegue en Render

**Para los dos servicios:**

Build Command:
```bash
pip install -r requirements.txt
```

Start Command de la API:
```bash
uvicorn app:app --host 0.0.0.0 --port $PORT
```

Start Command de Streamlit:
```bash
streamlit run streamlit_app.py --server.port $PORT --server.address 0.0.0.0 --server.headless true
```

La URL pública de la API se configura en `streamlit_app.py`, en la variable `API_URL`.

## Documentación de la API

**URL base:** https://opia-practica-3.onrender.com

### `POST /predict`

Recibe un JSON con las ocho variables necesarias para la predicción y devuelve el valor estimado de la vivienda en cientos de miles de dólares.

**Ejemplo de petición:**

```json
{
  "MedInc": 3.5,
  "HouseAge": 29.0,
  "AveRooms": 5.2,
  "AveBedrms": 1.05,
  "Population": 1166.0,
  "AveOccup": 2.8,
  "Latitude": 34.26,
  "Longitude": -118.49
}
```

**Ejemplo de respuesta (200):**

```json
{
  "MedHouseVal": 1.5126018234536092
}
```

**Errores:**

| Código | Cuándo ocurre | Qué muestra la aplicación |
|---|---|---|
| 422 | Falta un campo o tiene un tipo incorrecto. | Mensaje con el error de la API. |
| Sin respuesta | La API no está disponible o tarda demasiado. | Mensaje indicando que no se ha podido conectar con la API. |