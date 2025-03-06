# **📌 Implementación de Relaciones en MongoDB**

MongoDB, al ser una **base de datos NoSQL**, **no maneja relaciones de la misma manera que SQL**. Sin embargo, permite representar relaciones entre documentos utilizando dos enfoques principales:

1️⃣ **Relaciones mediante documentos embebidos (Denormalización - "Embeddeding")**  
2️⃣ **Relaciones mediante referencias (Normalización - "Referencing")**  

Cada enfoque tiene ventajas y desventajas según el caso de uso.

---

## **🔹 1. Relación con Documentos Embebidos (Denormalización)**
Este enfoque **anida los datos relacionados dentro del mismo documento**.  
Se usa cuando los datos relacionados se consultan frecuentemente juntos y no crecen demasiado en tamaño.

### **Ejemplo: Cliente con sus Pedidos embebidos**
```json
{
  "_id": 1,
  "nombre": "Juan",
  "email": "juan@email.com",
  "pedidos": [
    { "producto": "Laptop", "total": 1200 },
    { "producto": "Mouse", "total": 25 }
  ]
}
```

✅ **Ventajas**  
✔ Rápida recuperación de datos (no requiere `JOINs`).  
✔ Menos consultas a la base de datos.  
✔ Buena opción si los datos no crecen demasiado.  

❌ **Desventajas**  
✖ Si los pedidos crecen mucho, el documento se hace muy grande.  
✖ No se pueden actualizar pedidos de forma independiente sin modificar el cliente.  

---

## **🔹 2. Relación con Referencias (Normalización)**
En este enfoque, los documentos **almacenan solo referencias (IDs) de documentos en otras colecciones**.  
Se usa cuando los datos son reutilizados en múltiples documentos o crecen mucho en tamaño.

### **Ejemplo: Cliente y Pedidos en colecciones separadas con referencia**
#### **Colección `clientes`**
```json
{
  "_id": 1,
  "nombre": "Juan",
  "email": "juan@email.com",
  "pedidos": [101, 102]  // Referencias a pedidos
}
```

#### **Colección `pedidos`**
```json
[
  { "_id": 101, "cliente_id": 1, "producto": "Laptop", "total": 1200 },
  { "_id": 102, "cliente_id": 1, "producto": "Mouse", "total": 25 }
]
```

✅ **Ventajas**  
✔ Evita documentos muy grandes.  
✔ Permite reutilizar datos sin duplicarlos.  
✔ Se pueden actualizar las referencias sin modificar el documento original.  

❌ **Desventajas**  
✖ Se necesitan consultas adicionales (`$lookup`) para traer los datos completos.  
✖ Puede ser más lento en consultas frecuentes.  

---

## **🔍 📌 Comparación de Enfoques**
| Característica | Embebido | Referenciado |
|--------------|---------|--------------|
| 🔹 Rendimiento | Mejor para lecturas rápidas | Mejor para estructuras grandes |
| 🔹 Facilidad de consulta | Más fácil, todo en un solo documento | Requiere `$lookup` para unir datos |
| 🔹 Tamaño del documento | Puede crecer mucho | Más optimizado |
| 🔹 Modificación de datos | Más costoso, debe actualizar todo el documento | Más flexible, actualiza partes específicas |

📌 **Regla general**:  
- **Si los datos relacionados son de uso frecuente y pequeños →** Usa documentos embebidos.  
- **Si los datos crecen mucho o se usan en varias colecciones →** Usa referencias con `$lookup`.  

---

## **🔗 Relaciones en MongoDB con `$lookup`**
Si usaste **referencias** en lugar de embebidos, puedes usar `$lookup` para unir colecciones.

Ejemplo para obtener los clientes con sus pedidos referenciados:
```sh
db.clientes.aggregate([
  {
    "$lookup": {
      "from": "pedidos",
      "localField": "pedidos",
      "foreignField": "_id",
      "as": "detalles_pedidos"
    }
  }
])
```
📌 Esto traerá **los datos completos de los pedidos referenciados** en cada cliente.

---

## **🎯 ¿Qué enfoque usar en cada caso?**
| Caso de Uso | Recomendación |
|------------|--------------|
| Datos relacionados pequeños y consultados juntos | **Embebidos** |
| Datos que pueden crecer mucho | **Referencias** |
| Datos reutilizados en múltiples documentos | **Referencias** |
| Rendimiento en consultas es prioridad | **Embebidos** |
| Se necesita actualizar datos frecuentemente | **Referencias** |

---

## **🚀 Conclusión**
- MongoDB **no usa `JOINs` directamente**, pero puedes hacer relaciones con documentos **embebidos o referenciados**.  
- **Embebidos** → Son mejores para consultas rápidas y estructuras pequeñas.  
- **Referenciados** → Son mejores cuando los datos crecen mucho o se usan en varios lugares.  
- Para unir colecciones referenciadas, usa **`$lookup`**.  

Si necesitas más ejemplos o un caso específico, dime. 😊🚀