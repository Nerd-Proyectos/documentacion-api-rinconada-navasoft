# 📚 Documentación API - Cuenta (Sistema Rinconada - Navasoft)
---

## 📥 1. Obtener tipos de comprobante

**Método:** `GET`  
**Ruta:** `/cuentas/tipos-comprobantes`

**Descripción:**  
Obtener lista de tipos de comprobantes

**Ejemplo de llamada:**
```
GET /cuentas/tipos-comprobantes
```

### 🔸 Respuesta JSON:
```json
[
  {
    "cod": "01",
    "descripcion": "Boleta",
  },
  {
    "cod": "02",
    "descripcion": "Factura",
  },
  {
    "cod": "03",
    "descripcion": "Anticipo",
  },
]
```
---

## 📥 2. Obtener categorias de pagos

**Método:** `GET`  
**Ruta:** `/cuentas/categorias-pagos`

**Descripción:**  
Obtener lista de categorias de pagos

**Ejemplo de llamada:**
```
GET /cuentas/categorias-pagos
```

### 🔸 Respuesta JSON:
```json
[
  {
    "cod": "01",
    "descripcion": "PAGADO",
  },
  {
    "cod": "02",
    "descripcion": "VIGENTE",
  },
  {
    "cod": "03",
    "descripcion": "VENCIDO",
  },
]
```
---

## 📥 3. Obtener conceptos de comprobantes

**Método:** `GET`  
**Ruta:** `/cuentas/coceptos-comprobantes`

**Descripción:**  
Obtener lista de conceptos de comprobantes


**Ejemplo de llamada:**
```
GET /cuentas/coceptos-comprobantes
```

### 🔸 Respuesta JSON:
```json
[
  {
    "cod": "59",
    "descripcion": "Cuota de mantenimiento",
  }
]
```
---

## 📥 4. Obtener estado cuenta del socio 

**Método:** `GET`  
**Ruta:** `/cuentas/{codsoc}`

**Descripción:**  
Obtener el historial de pagos realizados por el socio en Navasoft, incluyendo conceptos, números de comprobantes, entre otros. Los datos se deben ordenar desde los más recientes hasta los más antiguos.

### 🔹 Parámetros:

| Parámetro | Tipo    | Requerido | Descripción                                                     |
|-----------|---------|-----------|-----------------------------------------------------------------|
| codigo    | string  | Opcional  | Lista de códigos de tipo de comprobantes separados por comas `,`|
| limit     | integer | Opcional  | Límite máximo de registros a devolver                           |

**Ejemplo de llamada:**
```
GET /cuentas/0001?codigo=58,59&limit=200
```

### 🔸 Respuesta JSON:
```json
[
  {
    "codigo" : "0001", // codigo interno de navasoft esto para no insertar datos repetidos
    "estado": "PAGADO",
    "codconcepto": "59",
    "desconcepto": "Cuota de Mantenimiento",
    "cod_tipo_comprobantre": "01",
    "tipo_comprobantre": "Boleta",
    "num_comprobante": "F001-00002",
    "fecha_emision": "2024-02-21",
    "fecha_vencimiento": "2024-02-28",
    "moneda": "S/",
    "importe": "320.00",
  },
  {
    "codigo" : "0002",
    "estado": "VENCIDO",
    "codconcepto": "59",
    "desconcepto": "Cuota de Mantenimiento",
    "cod_tipo_comprobantre": "01",
    "tipo_comprobantre": "Boleta",
    "num_comprobante": "F001-00002",
    "fecha_emision": "2024-02-21",
    "fecha_vencimiento": "2024-02-28",
    "moneda": "S/",
    "importe": "320.00",
  },
  {
    "codigo" : "0003",
    "estado": "VIGENTE",
    "codconcepto": "59",
    "desconcepto": "Cuota de Mantenimiento",
    "cod_tipo_comprobantre": "01",
    "tipo_comprobantre": "Boleta",
    "num_comprobante": "F001-00002",
    "fecha_emision": "2024-02-21",
    "fecha_vencimiento": "2024-02-28",
    "moneda": "$",
    "importe": "150.00",
  },
]
```
---

## 📥 5. Guardar pago realizado desde rinconada  a Navasoft (Como por ejemplo en las reserva de parrilla)

**Método:** `POST`  
**Ruta:** `/cuentas/guardar-pago`

**Descripción:**  
Endpoint que se debe utlizar para guadar pagos en navasoft desde sistema web de rinconada. Navasoft se encargaria de factura y retornar los datos del comprobante generado para actualizar de vuelta estos datos en el sitema de Rinconada.

### Datos que enviaremos

| Campo              | Descripción                                                  |
|--------------------|--------------------------------------------------------------|
| `codsoc`           | Código del socio titular                                     |
| `codconcepto`      | Código de concepto del pago                                  |
| `desconcepto`      | Apellido paterno del familiar                                |
| `fecha_pago`       | Fecha de pago (formato `YYYY-MM-DD`)                         |
| `modena`           | Moneda de pago S/ o $                                        |
| `importe`          | Importe de pago                                              |

**Ejemplo de llamada:**
```
GET /cuentas/guardar-pago
```

### Ejemplo de Respuesta Exitosa (200):
```json
{
    "status": 200,
    "message": "Pago registrado exitosamente",
    "data": {
        "codigo" : "0001", //Codigo de pago generado en navasoft
        "cod_tipo_comprobantre": "02",
        "tipo_comprobantre": "Factura",
        "num_comprobante": "SB01-0001000-2025"
    }
}
```

### Ejemplo de Respuesta con Error (422):
```json
{
    "status": 422,
    "message": "Error al registrar el pago"
}
```