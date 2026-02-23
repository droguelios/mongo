# mongo
# 🍃 MongoDB desde la Terminal — Guía Básica

## ¿Qué es MongoDB?

MongoDB es una base de datos **NoSQL documental**. En lugar de guardar datos en tablas con filas y columnas (como SQL), guarda datos en **documentos** con formato similar a JSON.

---

## SQL vs MongoDB — Comparación rápida

| SQL (PostgreSQL) | MongoDB |
|---|---|
| Tablas | Colecciones |
| Filas | Documentos |
| Columnas | Campos |
| JOINs | Embedding / Referencias |
| Schema rígido | Schema flexible |

---

## Conectarte a MongoDB

```bash
mongosh
```

Verás algo así:

```
Current Mongosh Log ID: ...
Connecting to: mongodb://localhost:27017
test>
```

---

## Navegación básica

```bash
# Ver todas las bases de datos
show dbs

# Crear o entrar a una base de datos
use saludplus

# Ver en qué base de datos estás
db

# Ver las colecciones que existen
show collections
```

> ⚠️ MongoDB **no crea la base de datos** hasta que insertas algo en ella.

---

## CRUD — Crear, Leer, Actualizar, Eliminar

### 📝 Insertar documentos

```bash
# Insertar uno
db.patients.insertOne({
  name: "Valeria Gomez",
  email: "valeria@mail.com",
  age: 30
})

# Insertar varios
db.patients.insertMany([
  { name: "Carlos López", email: "carlos@mail.com" },
  { name: "Ana Torres",   email: "ana@mail.com" }
])
```

---

### 🔍 Leer documentos

```bash
# Traer todos
db.patients.find()

# Con formato legible
db.patients.find().pretty()

# Buscar por un campo
db.patients.find({ email: "valeria@mail.com" })

# Traer solo uno
db.patients.findOne({ email: "valeria@mail.com" })
```

---

### ✏️ Actualizar documentos

```bash
# Actualizar un campo
db.patients.updateOne(
  { email: "valeria@mail.com" },    # ← quién
  { $set: { name: "Valeria G." } }  # ← qué cambiar
)

# Actualizar varios
db.patients.updateMany(
  { age: 30 },
  { $set: { grupo: "adulto" } }
)
```

---

### 🗑️ Eliminar documentos

```bash
# Eliminar uno
db.patients.deleteOne({ email: "valeria@mail.com" })

# Eliminar varios
db.patients.deleteMany({ age: 30 })

# Eliminar TODOS (¡cuidado!)
db.patients.deleteMany({})
```

---

## Flujo completo de práctica

Copia y pega esto paso a paso en tu terminal:

```bash
# 1. Entrar a la base de datos
use saludplus

# 2. Insertar un documento
db.patients.insertOne({ name: "Valeria", email: "v@mail.com" })

# 3. Ver lo que se insertó
db.patients.find()

# 4. Actualizar el nombre
db.patients.updateOne({ email: "v@mail.com" }, { $set: { name: "Valeria Gomez" } })

# 5. Verificar el cambio
db.patients.findOne({ email: "v@mail.com" })

# 6. Eliminar el documento
db.patients.deleteOne({ email: "v@mail.com" })
```

---

## Conceptos clave

### Embedding vs Referencias

**Embedding** — meter los datos dentro del mismo documento:

```json
{
  "patientEmail": "valeria@mail.com",
  "appointments": [
    { "date": "2024-01-07", "doctor": "Dr. Carlos Ruiz" },
    { "date": "2024-03-15", "doctor": "Dr. Ana López" }
  ]
}
```

✅ Lectura rapidísima, todo en una consulta  
❌ Si el doctor cambia su nombre, hay que actualizar muchos documentos

---

**Referencias** — guardar solo el ID:

```json
{
  "patientEmail": "valeria@mail.com",
  "appointments": ["APT-1001", "APT-1002"]
}
```

✅ Datos siempre actualizados  
❌ Necesitas hacer varias consultas

---

### Índices

Le dicen a MongoDB por dónde buscar rápido:

```bash
db.patients.createIndex({ email: 1 })
```

- Sin índice → recorre **todos** los documentos (lento 🐢)
- Con índice → va **directo** al documento (rápido ⚡)

---

## ¿Cuándo usar MongoDB?

| ✅ Úsalo cuando... | ❌ Evítalo cuando... |
|---|---|
| Lees documentos completos frecuentemente | Necesitas transacciones estrictas (pagos) |
| El schema puede cambiar | Los datos tienen muchas relaciones |
| Quieres evitar JOINs costosos | Necesitas consistencia absoluta entre entidades |

---

*Guía elaborada para el módulo M4 - Cohorte 6 · SaludPlus*
