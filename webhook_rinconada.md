
# 📡 Webhook de Creación y Actualización de Socios - Rinconada

Este webhook permite la creación y actualización de **socios titulares** y **familiares** en el sistema de Rinconada. La comunicación se realiza vía `POST` a través de una URL especificada y autenticada mediante un header.

---

## 🔗 URL del Webhook

```
POST https://api.rinconada.com.pe/api/webhook/socios
```

### Headers Requeridos

| Header                     | Valor                                                    |
|---------------------------|----------------------------------------------------------|
| `X-Webhook-Secret-Rinconada` |                                                       |
| `Content-Type`            | `application/json`                                      |

---

## 🎯 Eventos Disponibles

- `phatner.created`: Crea un nuevo socio (titular o familiar).
- `phatner.updated`: Actualiza la información de un socio existente.

---

## 🧾 Campos y Validaciones

### 🎓 Reglas para creación (`phatner.created`)

| Campo                  | Tipo de Dato | Requerido | Validaciones adicionales                          |
|------------------------|--------------|-----------|--------------------------------------------------|
| tipo_doc               | string       | Sí        | DNI/CE/P                                         |
| nro_doc                | numérico     | Sí        | Único en `users.nro_doc`                         |
| codigo_socio           | string       | Sí        | Único en `users.CodSoc`                          |
| codigo_socio_principal | string       | Opcional  | Solo para familiares                             |
| categoria_socio        | string       | Sí        | TITULAR o FAMILIAR                               |
| codigo_cliente         | string       | Sí        |                                                  |
| codigo_familiar        | string       | Opcional  | Máx: 50 caracteres                               |
| parentezco             | string       | Opcional  | Máx: 50 caracteres                               |
| nombres                | string       | Sí        | Máx: 255 caracteres                              |
| apellido_pa            | string       | Sí        | Máx: 255 caracteres                              |
| apellido_ma            | string       | Sí        | Máx: 255 caracteres                              |
| nacionalidad           | string       | Opcional  | Máx: 100 caracteres                              |
| lugar_nac              | string       | Opcional  | Máx: 255 caracteres                              |
| edo_civil              | string       | Opcional  | Máx: 50 caracteres                               |
| sexo                   | string       | Opcional  | Máx: 10 caracteres                               |
| fech_nac               | fecha        | Opcional  | Formato YYYY-MM-DD                               |
| edad                   | entero       | Opcional  | Mínimo 0                                         |
| direccion              | string       | Opcional  | Máx: 255 caracteres                              |
| ubicacion              | string       | Opcional  | Máx: 255 caracteres                              |
| tel_fijo               | string       | Opcional  | Máx: 20 caracteres                               |
| tel_cel                | string       | Opcional  | Máx: 20 caracteres                               |
| email                  | email        | Opcional  | Formato válido                                   |
| centro_estudio         | string       | Opcional  | Máx: 255 caracteres                              |
| especialidad           | string       | Opcional  | Máx: 255 caracteres                              |
| profesion              | string       | Opcional  | Máx: 255 caracteres                              |
| telefono_empresa       | string       | Opcional  | Máx: 20 caracteres                               |
| centro_laboral         | string       | Opcional  | Máx: 255 caracteres                              |
| fax_empresa            | string       | Opcional  | Máx: 20 caracteres                               |
| cargo_laboral          | string       | Opcional  | Máx: 255 caracteres                              |
| dir_empresa            | string       | Opcional  | Máx: 255 caracteres                              |

---

### 🔄 Reglas para actualización (`phatner.updated`)

- Solo se requiere el `codigo_socio` como identificador.
- El resto de los campos es opcional, pero si se envían, deben cumplir con las validaciones anteriores.
- El `email` debe ser único

---

## 📥 Ejemplo de Payloads

### ✅ Creación de Socio Titular

```json
{
  "event": "phatner.created",
  "data": {
    "tipo_doc": "DNI",
    "nro_doc": "99992220",
    "codigo_socio": "SOC123456",
    "categoria_socio": "TITULAR",
    "codigo_cliente": "CLI987654",
    "nombres": "Jose",
    "apellido_pa": "Pérez",
    "apellido_ma": "García",
    "nacionalidad": "Peruana",
    "fech_nac": "1990-05-15",
    "tel_cel": "987654321"
  }
}
```

### 👨‍👩‍👧‍👦 Creación de Socio Familiar

```json
{
  "event": "phatner.created",
  "data": {
    "tipo_doc": "DNI",
    "nro_doc": "99992221",
    "codigo_socio": "SOC123456C",
    "codigo_socio_principal": "SOC123456",
    "categoria_socio": "FAMILIAR",
    "codigo_cliente": "CLI987654",
    "codigo_familiar": "FC123456C",
    "parentezco": "CONYUGE",
    "nombres": "Maria",
    "apellido_pa": "Pérez",
    "apellido_ma": "García"
  }
}
```


### ℹ️ Nota Importante

Los ejemplos de payloads muestran los datos mínimos requeridos para cada evento. Sin embargo, también es posible incluir los campos opcionales según sea necesario, siempre y cuando cumplan con las validaciones especificadas.


### ♻️ Actualización de Socio

```json
{
  "event": "phatner.updated",
  "data": {
    "codigo_socio": "SOC123456",
    "nombres": "Jose Modificado",
    "email": "jose.modificado@example.com"
  }
}
```

### ℹ️ Nota Importante

Los ejemplos de payloads muestran los datos mínimos requeridos para cada evento. Sin embargo, también es posible incluir los campos opcionales según sea necesario, siempre y cuando cumplan con las validaciones especificadas.

---

## ✅ Respuestas del Webhook

### Éxito

```json
{
  "type": "success",
  "message": "Socio creado exitosamente.",
  "data": {}
}
```

### Error General

```json
{
  "type": "error",
  "message": "Error al procesar la solicitud."
}
```

### Error de Validación

```json
{
  "type": "error",
  "message": "Datos inválidos.",
  "errors": {
    "email": ["El campo email ya ha sido registrado."]
  }
}
```

---

## 🔐 Notas de Seguridad

- Asegúrese de validar el header `X-Webhook-Secret-Rinconada` en cada solicitud.
