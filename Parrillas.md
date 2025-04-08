# 📚 Documentación API - Cuenta (Sistema Rinconada - Parrillas)
---


## 📥 0. Obtener categoria de invitados

**Método:** `GET`  
**Ruta:** `/invitados/categorias`

**Descripción:**  
Obtiene la lista de categorias de invitados

**Ejemplo de llamada:**
```
GET /invitados/categorias
```

### 🔸 Respuesta JSON:
```json
[
  {
    "cod": "01",
    "descripcion": "Adulto",
    "monto": 30.00
  },
  {
    "cod": "02",
    "descripcion": "Niño",
    "monto": 15.00
  },
]
```
---


## 📥 1. Obtener lista de parrillas

**Método:** `GET`  
**Ruta:** `/parrillas/listas`

**Descripción:**  
Obtener lista de  parrillas registradas en navasoft

**Ejemplo de llamada:**
```
GET /parrillas/listas
```

### 🔸 Respuesta JSON:
```json
[
  {
    "cod": "01",
    "lugar": "Los Paltos",
    "descripcion": "Parrilla 01",
    "aforo": 20,
  },
  {
    "cod": "02",
    "lugar": "Los Paltos",
    "descripcion": "Parrilla 02",\
    "aforo": 15,
  },
  {
    "cod": "03",
    "lugar": "Los Paltos",
    "descripcion": "Parrilla 03",
    "aforo": 25,
  },
]
```
---

## 📥 2. Guardar reserva de parrilla realizada en rinconada web hacia navasoft

**Método:** `POST`  
**Ruta:** `/parrillas/reservar`

**Descripción:**  
Este endpoint se utiliza para guardar reservas de parrillas que se han realizado en la web de Rinconada hacia Navasoft.

### Datos que enviaremos

| Campo              | Descripción                                                  |
|--------------------|--------------------------------------------------------------|
| `codsoc`           | Código del socio que hace reserva                            |
| `cod_parrilla`     | Código de parrilla seleccionada por el socio                 |
| `fecha_reserva`    | Fecha que el usuario ha reservado                            |

**Ejemplo de llamada:**
```
POST /parrillas/reservar
```

### Ejemplo de Respuesta Exitosa (200):
```json
{
    "status": 200,
    "message": "Reserva registrada exitosamente",
    "data": {
        "codigo" : "0001", //coodigo unico de reserva generado por navasoft
    }
}
```

### Ejemplo de Respuesta con Error (422):
```json
{
    "status": 422,
    "message": "Error al registrar la reserva"
}
```


## 📥 3. Agregar invitados a reserva

**Método:** `POST`  
**Ruta:** `/parrillas/reserva/{codigo_reserva}/invitados`

Este endpoint se utiliza para agregar los invitados a la reserva de parrilla creada anteriormente.

### Datos que enviaremos

| Campo              | Descripción                                                  |
|--------------------|--------------------------------------------------------------|
| `codsoc`           | Código del socio que hace reserva                            |
| `codido_reserva`   | Código de la reserva devuelto por navasoft                   |
| `invitados`        | Este campo es un arreglo con la lista de invitados           |

### Descripcion de campos de invitados

| Campo              | Descripción                                                  |
|--------------------|--------------------------------------------------------------|
| `numero_documento` | Código de la reserva devuelto por navasoft                   |
| `nombre_completo`  | Código de la reserva devuelto por navasoft                   |
| `codigo_categoria` | Codigo de categoria de invitados                             |
| `invitado_gratis`  | 1: SI, 0 NO  (Indica si el invitado es gratuito)             |
| `monto_pagado`     | Monto pagado por invitado                                    |

**Ejemplo de llamada:**
```
GET /parrillas/reserva/R67890/invitados
```

**Ejemplo de datos enviados**

```json
{
  "codsoc": "S12345",
  "codido_reserva": "R67890",
  "invitados": [
    {
      "numero_documento": "12345678",
      "nombre_completo": "Juan Pérez",
      "categoria": "Adulto",
      "invitado_gratis": 0,
      "monto_pagado": 15.00
    },
    {
      "numero_documento": "87654321",
      "nombre_completo": "Martina Gómez",
      "categoria": "Niño",
      "invitado_gratis": 1,
      "monto_pagado": 0.00
    }
  ]
}
```

### Ejemplo de Respuesta Exitosa (200):
```json
{
    "status": 200,
    "message": "Invitados registrados exitosamente",
}
```

### Ejemplo de Respuesta con Error (422):
```json
{
    "status": 422,
    "message": "Error al registrar los invitados"
}
```