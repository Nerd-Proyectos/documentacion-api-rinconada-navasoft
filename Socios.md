
# 📚 Documentación API - Socios (Sistema Rinconada - Navasoft)
---

## 📥 1. Obtener Información de Socios

**Método:** `GET`  
**Ruta:** `/socios`

**Descripción:**  
Obtiene la lista de socios registrados, permitiendo filtrar por estados y cantidad máxima de resultados.

### 🔹 Parámetros:

| Parámetro | Tipo    | Requerido | Descripción                                          |
|-----------|---------|-----------|------------------------------------------------------|
| estados   | string  | Opcional  | Lista de códigos de estado separados por comas `,`   |
| limit     | integer | Opcional  | Límite máximo de registros a devolver                |

**Ejemplo de llamada:**
```
GET /socios?estados=000,0001,0002&limit=200
```

### 🔸 Respuesta JSON:
```json
[
  {
    "codsoc": "00123",
    "codcli": "04567",
    "nomcli": "Juan Pérez",
    "apellido_paterno": "Pérez",
    "apellido_materno": "García",
    "nombres": "Juan",
    "tipo_doc": "DNI",
    "nro_doc": "12345678",
    "estado": "habilitado",
    "nomcat": "Activo",
    "nacionalidad": "Peruana",
    "lugar_nacimiento": "Lima",
    "edo_civil": "Soltero",
    "sexo": "M",
    "fecha_nacimiento": "1990-05-10",
    "direccion": "Av. Siempre Viva 123",
    "ubigeo": "150101",
    "tel_fijo": "012345678",
    "tel_cel": "987654321",
    "email": "juan.perez@email.com",
    "centro_estudios": "Universidad de Lima",
    "especialidad": "Ingeniería",
    "profesion": "Ingeniero",
    "telefono_empresa": "012233344",
    "centro_laboral": "Empresa S.A.",
    "fax_empresa": "012233355",
    "cargo_laboral": "Jefe de Proyecto",
    "dir_empresa": "Av. Principal 456",
    "foto": "base64string"
  }
]
```

---

## 📤 2. Actualizar Datos de un Socio

**Método:** `PUT`  
**Ruta:** `/socios/{codsoc}/update`

**Descripción:**  
Permite actualizar los datos personales de un socio usando su `codsoc`.

### 🔹 Datos que se envían:

| Campo             | Tipo    | Requerido | Descripción                           |
|-------------------|---------|-----------|---------------------------------------|
| codsoc            | string  | Sí        | Código del socio (identificador)      |
| apellido_paterno  | string  | Sí        | Nuevo apellido paterno                |
| apellido_materno  | string  | Sí        | Nuevo apellido materno                |
| nombres           | string  | Sí        | Nuevos nombres                        |
| tipo_doc          | string  | No        | Tipo de documento (DNI, Pasaporte)    |
| nro_doc           | string  | No        | Número de documento                   |
| estado            | string  | No        | Estado del socio                      |
| nacionalidad      | string  | No        | Nacionalidad                          |
| lugar_nacimiento  | string  | No        | Lugar de nacimiento                   |
| edo_civil         | string  | No        | Estado civil                          |
| sexo              | string  | No        | Sexo                                  |
| fecha_nacimiento  | string  | No        | Fecha de nacimiento (YYYY-MM-DD)      |
| direccion         | string  | No        | Dirección                             |
| ubigeo            | string  | No        | Ubigeo                                |
| tel_fijo          | string  | No        | Teléfono fijo                         |
| tel_cel           | string  | No        | Teléfono celular                      |
| email             | string  | No        | Correo electrónico                    |
| centro_estudios   | string  | No        | Centro de estudios                    |
| especialidad      | string  | No        | Especialidad                          |
| profesion         | string  | No        | Profesión                             |
| telefono_empresa  | string  | No        | Teléfono empresa                      |
| centro_laboral    | string  | No        | Centro laboral                        |
| fax_empresa       | string  | No        | Fax empresa                           |
| cargo_laboral     | string  | No        | Cargo laboral                         |
| dir_empresa       | string  | No        | Dirección empresa                     |
| foto              | string  | No        | Foto en base64                        |

---

**Ejemplo de respuesta 200**
```json{
    "status": 200,
    "message": "Actualizado correctamente"
}
```

**Ejemplo de respuesta 422**    
```json{
    "status": 422,
    "message": "Error al actualizar los datos",
    "errors": [
        {
            "field": "nro_doc",
            "error": "El número de documento es inválido"
        },
        {
            "field": "email",
            "error": "El formato del correo electrónico es incorrecto"
        }
    ]
}

```
---

---

## 📥 3. Obtener Información de Familiares de Socios

**Método:** `GET`  
**Ruta:** `/socios/familiares`

**Descripción:**  
Obtiene los datos de los familiares asociados a los socios.

### 🔹 Parámetros:

| Parámetro | Tipo    | Requerido | Descripción                                           |
|-----------|---------|-----------|-------------------------------------------------------|
| codsoc    | string  | Requerido | Código del socio para listar sus familiares           |

**Ejemplo de llamada:**
```
GET /socios/familiares?codsoc=00123
```

### 🔸 Respuesta JSON:
```json
[
  {
    "codsoc": "00123",
    "codfam": "F001",
    "parentesco": "Hijo",
    "apellido_paterno": "Pérez",
    "apellido_materno": "García",
    "nombres": "Carlos",
    "tipo_doc": "DNI",
    "nro_doc": "87654321",
    "fecha_nacimiento": "2010-03-15",
    "sexo": "M",
    "estado_civil": "Soltero",
    "nacionalidad": "Peruana"
  }
]
```


## 📥 4. Actualiza los dartos de un familiar 

# Endpoint para Actualizar Datos de Familiares

**Método:** `PUT`  
**Endpoint:** `/socios/{codsoc}/familiares/{codfam}/update`

**Descripción:**  
Actualiza la información de un **familiar** en Navasoft usando el **código del socio titular** (`codsoc`) y el **código del familiar** (`codfam`).

### Datos que debes enviar:

| Campo              | Descripción                                                  |
|--------------------|--------------------------------------------------------------|
| `codsoc`           | Código del socio titular (no se actualiza)                   |
| `codfam`           | Código del familiar (no se actualiza)                        |
| `apellido_paterno` | Apellido paterno del familiar                                |
| `apellido_materno` | Apellido materno del familiar                                |
| `nombres`          | Nombres del familiar                                          |
| `tipo_doc`         | Tipo de documento                                             |
| `nro_doc`          | Número de documento                                           |
| `nacionalidad`     | Nacionalidad                                                  |
| `lugar_nacimiento` | Lugar de nacimiento                                           |
| `edo_civil`        | Estado civil                                                  |
| `sexo`             | Sexo (M/F)                                                    |
| `fecha_nacimiento` | Fecha de nacimiento (formato `YYYY-MM-DD`)                   |
| `direccion`        | Dirección del familiar                                        |
| `tel_fijo`         | Teléfono fijo                                                 |
| `tel_cel`          | Teléfono celular                                              |
| `email`            | Correo electrónico                                            |
| `foto`             | Foto (opcional)                                               |

### Ejemplo de Request:
```http
PUT /socios/00123/familiares/00045/update
Content-Type: application/json

{
    "codsoc": "00123",
    "codfam": "00045",
    "apellido_paterno": "Gómez",
    "apellido_materno": "Ramírez",
    "nombres": "Lucía",
    "tipo_doc": "DNI",
    "nro_doc": "87654321",
    "nacionalidad": "Peruana",
    "lugar_nacimiento": "Arequipa",
    "edo_civil": "Soltera",
    "sexo": "F",
    "fecha_nacimiento": "2000-01-15",
    "direccion": "Calle Falsa 123",
    "tel_fijo": "054123456",
    "tel_cel": "987654321",
    "email": "lucia.gomez@example.com",
    "foto": "base64string"
}
```

### Ejemplo de Respuesta Exitosa (200):
```json
{
    "status": 200,
    "message": "Actualizado correctamente"
}
```

### Ejemplo de Respuesta con Error (422):
```json
{
    "status": 422,
    "message": "Error al actualizar los datos",
    "errors": [
        {
            "field": "nro_doc",
            "error": "El número de documento es inválido"
        },
        {
            "field": "email",
            "error": "El formato del correo electrónico es incorrecto"
        }
    ]
}
```

