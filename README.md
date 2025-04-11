# PROYECTO-HOTEL-CDE

# **Documentación del Backend - Sistema de Gestión de Reservas**

## **Descripción General**
Este backend permite gestionar reservas de un hotel, incluyendo la creación, actualización, eliminación y consulta de reservas. Utiliza Node.js con Express y una base de datos SQL Server.

---

## **Base URL**
```
http://localhost:5000/api/reservas
```

---

## **Autenticación**
- Todas las rutas protegidas requieren un token JWT.
- El token debe enviarse en el encabezado de la solicitud:
  - **Key:** `Authorization`
  - **Value:** `Bearer <tu_token_jwt>`

---

## **Endpoints Disponibles**

### **1. Obtener todas las reservas**
- **URL:** `/`
- **Método:** `GET`
- **Descripción:** Devuelve una lista de todas las reservas.
- **Autenticación:** No requerida.
- **Ejemplo de respuesta:**
  ```json
  [
    {
      "id": 1,
      "huesped_id": 1,
      "fecha_entrada": "2025-04-15T14:00:00.000Z",
      "fecha_salida": "2025-04-20T12:00:00.000Z",
      "sena": 100.0,
      "notas": "Reserva de prueba"
    }
  ]
  ```

---

### **2. Obtener una reserva por ID**
- **URL:** `/:id`
- **Método:** `GET`
- **Descripción:** Devuelve los detalles de una reserva específica.
- **Autenticación:** No requerida.
- **Parámetros:**
  - `id` (en la URL): ID de la reserva.
- **Ejemplo de respuesta:**
  ```json
  {
    "id": 1,
    "huesped_id": 1,
    "fecha_entrada": "2025-04-15T14:00:00.000Z",
    "fecha_salida": "2025-04-20T12:00:00.000Z",
    "sena": 100.0,
    "notas": "Reserva de prueba"
  }
  ```

---

### **3. Crear una nueva reserva**
- **URL:** `/`
- **Método:** `POST`
- **Descripción:** Crea una nueva reserva.
- **Autenticación:** Requerida.
- **Cuerpo de la solicitud:**
  ```json
  {
    "huesped_id": 1,
    "fecha_entrada": "2025-04-15T14:00:00.000Z",
    "fecha_salida": "2025-04-20T12:00:00.000Z",
    "sena": 100.0,
    "notas": "Reserva de prueba"
  }
  ```
- **Ejemplo de respuesta:**
  ```json
  {
    "message": "Reserva creada exitosamente",
    "reserva": {
      "id": 1,
      "huesped_id": 1,
      "fecha_entrada": "2025-04-15T14:00:00.000Z",
      "fecha_salida": "2025-04-20T12:00:00.000Z",
      "sena": 100.0,
      "notas": "Reserva de prueba"
    }
  }
  ```

---

### **4. Actualizar una reserva**
- **URL:** `/:id`
- **Método:** `PUT`
- **Descripción:** Actualiza los datos de una reserva existente.
- **Autenticación:** Requerida.
- **Parámetros:**
  - `id` (en la URL): ID de la reserva.
- **Cuerpo de la solicitud (campos opcionales):**
  ```json
  {
    "huesped_id": 1,
    "fecha_entrada": "2025-04-16T14:00:00.000Z",
    "fecha_salida": "2025-04-21T12:00:00.000Z",
    "sena": 150.0,
    "notas": "Reserva actualizada"
  }
  ```
- **Ejemplo de respuesta:**
  ```json
  {
    "message": "Reserva actualizada exitosamente",
    "reserva": {
      "id": 1,
      "huesped_id": 1,
      "fecha_entrada": "2025-04-16T14:00:00.000Z",
      "fecha_salida": "2025-04-21T12:00:00.000Z",
      "sena": 150.0,
      "notas": "Reserva actualizada"
    }
  }
  ```

---

### **5. Eliminar una reserva**
- **URL:** `/:id`
- **Método:** `DELETE`
- **Descripción:** Elimina una reserva específica.
- **Autenticación:** Requerida.
- **Parámetros:**
  - `id` (en la URL): ID de la reserva.
- **Ejemplo de respuesta:**
  ```json
  {
    "message": "Reserva eliminada exitosamente",
    "reserva": {
      "id": 1,
      "huesped_id": 1,
      "fecha_entrada": "2025-04-15T14:00:00.000Z",
      "fecha_salida": "2025-04-20T12:00:00.000Z",
      "sena": 100.0,
      "notas": "Reserva de prueba"
    }
  }
  ```

---

## **Validaciones**
- **Crear o actualizar una reserva (`POST` y `PUT`):**
  - `huesped_id`: Debe ser un número entero.
  - `fecha_entrada`: Debe ser una fecha válida en formato ISO 8601.
  - `fecha_salida`: Debe ser una fecha válida en formato ISO 8601 y posterior a `fecha_entrada`.
  - `sena`: Debe ser un número mayor o igual a 0.

---

## **Errores comunes**
- **400 Bad Request:**
  - Ocurre si los datos enviados no cumplen con las validaciones.
  - Ejemplo de respuesta:
    ```json
    {
      "errors": [
        {
          "msg": "El ID del huésped debe ser un número entero.",
          "param": "huesped_id",
          "location": "body"
        }
      ]
    }
    ```

- **401 Unauthorized:**
  - Ocurre si no se envía un token JWT válido en las rutas protegidas.
  - Ejemplo de respuesta:
    ```json
    {
      "message": "Acceso denegado. Token no proporcionado."
    }
    ```

- **404 Not Found:**
  - Ocurre si no se encuentra una reserva con el ID especificado.
  - Ejemplo de respuesta:
    ```json
    {
      "error": "Reserva no encontrada"
    }
    ```

---

