# 📚 Taller: Modelado NoSQL Documental con MongoDB para "Mis Recursos App"

<p align="center"> 
  <img src="https://media.tenor.com/MwLf-almaYEAAAAi/vibe-pepe-the-frog-vibe-swag-pepe-the-frog.gif" width="350"/> 
</p>

<p align="center"> 
  <img src="https://img.shields.io/badge/MongoDB-6.0+-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB">
  <img src="https://img.shields.io/badge/MQL-Query%20Language-brightgreen?style=for-the-badge&logo=mongodb" alt="MQL">
  <img src="https://img.shields.io/badge/Database-Documental-darkgreen?style=for-the-badge&logo=database&logoColor=white" alt="Documental">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="MIT License">
  <img src="https://img.shields.io/badge/Status-In%20Progress-yellowgreen?style=for-the-badge" alt="In Progress">
</p>

> 🧠 Exploramos cómo modelar los datos de una aplicación de gestión de recursos (series, películas, libros) usando una base de datos documental en lugar de un modelo relacional tradicional.

---

## 📚 Investigación

### ❓ ¿Qué es una base de datos NoSQL?

Una base de datos NoSQL (Not Only SQL) permite almacenar información en formatos no tabulares. Es ideal para sistemas que requieren flexibilidad en la estructura de los datos, alto rendimiento y escalabilidad horizontal.

### 🍃 ¿Qué es MongoDB?

MongoDB es una base de datos NoSQL orientada a documentos. Utiliza documentos BSON (muy similares a JSON) para representar y almacenar datos complejos, anidados y semiestructurados.

### ⚖️ Diferencias clave entre MySQL (Relacional) y MongoDB (Documental)

| Característica         | MySQL (Relacional)                                  | MongoDB (Documental)                                |
|:-----------------------|:----------------------------------------------------|:----------------------------------------------------|
| **Modelo de datos**    | Tablas con filas y columnas definidas.              | Colecciones con documentos BSON (similares a JSON). |
| **Esquema**            | Rígido y predefinido.                               | Flexible y dinámico.                                |
| **Relaciones**         | `JOIN`s a través de claves foráneas.                | Embebido de documentos o referencias (`$lookup`).   |
| **Escalabilidad**      | Vertical (aumentando la potencia del servidor).     | Horizontal (distribuyendo datos en más servidores). |
| **Lenguaje de Consulta** | SQL (`Structured Query Language`).                  | MQL (`MongoDB Query Language`) y Aggregation Pipeline.|
| **Casos de uso**       | Sistemas transaccionales, ERPs, contabilidad.       | Big Data, catálogos de productos, redes sociales.   |

### 📄 ¿Qué son documentos y colecciones?

- **Documento**: Es una unidad de datos en formato JSON (ej. un recurso o un usuario).
- **Colección**: Es un conjunto de documentos similares (ej. todos los recursos).

---

## 🧩 Diseño Detallado del Modelo

En lugar de normalizar como en SQL (con tablas separadas para usuarios, recursos, etc.), usaremos documentos para agrupar datos relacionados. El objetivo es equilibrar la flexibilidad con la eficiencia en las consultas, evitando redundancias excesivas.

### 🗂️ Colecciones Principales

- **`recursos`**: Almacena todos los recursos (series, películas, libros) con sus metadatos, estados y valoraciones. Cada documento representa un recurso completo con toda su información.

### ⚖️ Justificación: Embeber vs. Referenciar

La decisión clave en MongoDB es cuándo anidar datos (embeber) y cuándo crear un enlace (referenciar).

- **Embebemos** datos cuando la relación es de "contiene" y los datos no se consultan fuera de su documento padre.
  - **Ventaja**: Lecturas atómicas y rápidas (un solo viaje a la base de datos).
  - **Ejemplo**: Los datos del recurso están embebidos en un solo documento para consultas rápidas.

- **Referenciamos** datos cuando la relación es de "usa" o para evitar la duplicación de grandes volúmenes de datos que cambian con frecuencia.
  - **Ventaja**: Mantiene los datos consistentes (DRY - Don't Repeat Yourself).
  - **Ejemplo**: En futuras versiones, podríamos referenciar usuarios desde los recursos.

### 🧬 Estructura de Campos Clave

- **Campos de texto**: Para nombres, géneros, plataformas y reseñas.
- **Campos numéricos**: Para valoraciones (1-5 estrellas).
- **Campos de fecha**: Para fechas de creación, actualización y terminación.
- **Campos de estado**: Para el progreso del recurso (Pendiente, En progreso, Terminado).

---

## 📁 Estructura de Archivos

- **`recursos.json`** - Archivo JSON con 50 registros de ejemplo para la colección principal
- **`README.md`** - Este archivo de documentación

## 📦 Archivos JSON para Importación

### Colección Principal: `recursos`
- **Archivo:** `recursos.json`
- **Registros:** 50 recursos de ejemplo
- **Contenido:** Series, películas y libros con diferentes estados, plataformas y valoraciones

## 🗄️ Base de Datos

**Nombre de la Base de Datos:** `mis_recursos_app`

**Colección:** `recursos`

## 🛠️ Configuración de la Base de Datos

### 1. Conectar a MongoDB
```bash
mongosh
```

### 2. Crear y Usar la Base de Datos
Usar el comando para crear y seleccionar la base de datos `mis_recursos_app`

### 3. Crear la Colección
Crear la colección `recursos` para almacenar los documentos

### 4. Crear Índices para Optimizar Consultas
Crear índices en los campos principales para mejorar el rendimiento de las consultas:
- Índice para búsqueda por nombre
- Índice para filtros por estado
- Índice para filtros por formato
- Índice para filtros por plataforma
- Índice compuesto para búsquedas eficientes

### 5. Verificar la Creación
Verificar que la base de datos y colección se crearon correctamente

## 🧪 Ejemplos de Documentos JSON

### 📚 Recurso (Serie en Progreso)

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "nombre": "Breaking Bad",
  "genero": "Drama",
  "plataforma": "Netflix",
  "estado": "En progreso",
  "formato": "Serie",
  "fechaTerminacion": null,
  "valoracion": null,
  "reseña": "Serie muy intensa, me está gustando mucho. Walter White es un personaje fascinante.",
  "fechaCreacion": "2024-01-15T00:00:00.000Z",
  "fechaActualizacion": "2024-01-20T00:00:00.000Z"
}
```

### 🎬 Recurso (Película Terminada)

```json
{
  "_id": "507f1f77bcf86cd799439012",
  "nombre": "El Padrino",
  "genero": "Drama",
  "plataforma": "Amazon Prime",
  "estado": "Terminado",
  "formato": "Película",
  "fechaTerminacion": "2024-01-10T00:00:00.000Z",
  "valoracion": 5,
  "reseña": "Obra maestra del cine, perfecta en todos los aspectos. Marlon Brando es increíble.",
  "fechaCreacion": "2024-01-05T00:00:00.000Z",
  "fechaActualizacion": "2024-01-10T00:00:00.000Z"
}
```

### 📖 Recurso (Libro Pendiente)

```json
{
  "_id": "507f1f77bcf86cd799439013",
  "nombre": "1984",
  "genero": "Ciencia Ficción",
  "plataforma": "Kindle",
  "estado": "Pendiente",
  "formato": "Libro",
  "fechaTerminacion": null,
  "valoracion": null,
  "reseña": null,
  "fechaCreacion": "2024-01-18T00:00:00.000Z",
  "fechaActualizacion": "2024-01-18T00:00:00.000Z"
}
```

## 🚀 Funcionalidades Implementadas

### 1. CRUD (Crear, Leer, Actualizar, Eliminar)

✅ **Crear:** Añadir nuevos recursos con todos los campos requeridos
✅ **Leer:** Mostrar lista de todos los recursos
✅ **Actualizar:** Modificar detalles de recursos existentes
✅ **Eliminar:** Eliminar recursos de la base de datos

### 2. Filtros y Búsqueda

✅ **Filtro por Estado:** Terminado, En progreso, Pendiente
✅ **Filtro por Formato:** Serie, Película, Libro
✅ **Filtro por Plataforma:** Netflix, Amazon, HBO, etc.
✅ **Búsqueda por Nombre:** Búsqueda de texto con regex

### 3. Validaciones Básicas

✅ **Validación de Estados:** Solo valores permitidos
✅ **Validación de Formatos:** Solo valores permitidos
✅ **Validación de Valoración:** Entre 1 y 5 estrellas
✅ **Validación de Campos Requeridos:** Nombre, género, plataforma, estado, formato

## 📋 Cómo Usar

### 1. Configurar MongoDB

1. Asegúrate de tener MongoDB instalado y ejecutándose
2. Abre MongoDB Compass o la línea de comandos de MongoDB

### 2. Crear la Base de Datos

Crear y seleccionar la base de datos `mis_recursos_app`

### 3. Crear la Colección

Crear la colección `recursos` para almacenar los documentos

### 4. Crear Índices (Opcional pero Recomendado)

Crear índices en los campos principales para optimizar las consultas:
- Índice para búsqueda por nombre
- Índice para filtros por estado
- Índice para filtros por formato
- Índice para filtros por plataforma

### 5. Importar Datos JSON

#### Opción A: Usando MongoDB Compass
1. Abre MongoDB Compass
2. Conéctate a tu base de datos
3. Selecciona la base de datos `mis_recursos_app`
4. Selecciona la colección `recursos`
5. Haz clic en "Add Data" → "Import File"
6. Selecciona el archivo `recursos.json`
7. Configura las opciones de importación:
   - **Input File Type:** JSON
   - **Input Source:** File
   - **Import Mode:** Insert Documents
8. Haz clic en "Import"

#### Opción B: Usando línea de comandos
```bash
# Importar desde archivo JSON
mongoimport --db mis_recursos_app --collection recursos --file recursos.json --jsonArray
```

### 6. Verificar la Importación

Verificar que los datos se importaron correctamente contando los documentos y revisando algunos ejemplos

### 7. Probar las Funcionalidades

Ahora puedes probar las consultas básicas en MongoDB para:
- Ver todos los recursos
- Filtrar por estado
- Filtrar por formato
- Buscar por nombre
- Contar recursos

## Ejemplos de Operaciones

### Crear un Nuevo Recurso
Insertar un nuevo documento con los campos requeridos

### Buscar Recursos por Estado
Filtrar documentos por el campo estado

### Filtrar por Formato
Filtrar documentos por el campo formato

### Buscar por Nombre
Realizar búsquedas de texto en el campo nombre

### Actualizar un Recurso
Modificar campos específicos de un documento existente

## Índices Recomendados

Para optimizar las consultas, se recomiendan los siguientes índices:
- Índice en el campo nombre
- Índice en el campo estado
- Índice en el campo formato
- Índice en el campo plataforma

## 📊 Datos de Ejemplo Incluidos

El archivo `recursos.json` incluye **50 recursos de ejemplo** con una gran variedad de:

### 📺 Series (20 registros):
- Breaking Bad, Stranger Things, The Office, Friends, Game of Thrones
- The Crown, The Mandalorian, The Witcher, Black Mirror, The Handmaid's Tale
- Bridgerton, Money Heist, The Walking Dead, The Queen's Gambit
- Y más...

### 🎬 Películas (15 registros):
- El Padrino, El Señor de los Anillos, Inception, Pulp Fiction, Dune
- El Resplandor, Titanic, The Matrix, Jurassic Park, Forrest Gump
- Interstellar, The Godfather Part II, The Shawshank Redemption
- Y más...

### 📖 Libros (15 registros):
- 1984, Harry Potter, El Hobbit, El Código Da Vinci, Cien años de soledad
- El Principito, El Señor de las Moscas, El Gran Gatsby, Don Quijote
- El Alquimista, El Retrato de Dorian Gray
- Y más...

### 📈 Estados Distribuidos:
- **Terminado:** 25 recursos
- **En progreso:** 15 recursos  
- **Pendiente:** 10 recursos

### 🎯 Plataformas Incluidas:
- Netflix, Amazon Prime, HBO Max, Disney+, Hulu
- Kindle, Audible, Físico

### ⭐ Valoraciones:
- Recursos con valoraciones de 1 a 5 estrellas
- Recursos sin valoración (en progreso o pendientes)

Cada recurso tiene diferentes estados, plataformas y valoraciones para probar todas las funcionalidades de filtrado y búsqueda.

## 📝 Notas Importantes

1. **Nivel Básico:** Este proyecto está diseñado para principiantes en MongoDB
2. **Sin Aplicación Web:** Solo se incluye la estructura de datos
3. **Validaciones Simples:** Las validaciones son básicas y se pueden mejorar
4. **Escalable:** La estructura permite agregar más funcionalidades fácilmente

---

# 📚 Reflexión y Comparativa: Modelado Documental con MongoDB vs SQL Relacional

## 🧠 Comparativa Técnica

| Característica            | MySQL (Relacional)                                  | MongoDB (Documental)                                |
|:--------------------------|:----------------------------------------------------|:----------------------------------------------------|
| **Modelo de datos**       | Tablas con filas y columnas definidas.              | Colecciones con documentos BSON (similares a JSON). |
| **Esquema**               | Rígido y predefinido.                               | Flexible y dinámico.                                |
| **Relaciones**            | `JOIN`s a través de claves foráneas.                | Embebido de documentos o referencias (`$lookup`).   |
| **Escalabilidad**         | Vertical (aumentando la potencia del servidor).     | Horizontal (distribuyendo datos en más servidores). |
| **Lenguaje de Consulta**  | SQL (`Structured Query Language`).                  | MQL (`MongoDB Query Language`) y Aggregation Pipeline. |
| **Casos de uso**          | Sistemas transaccionales, ERPs, contabilidad.       | Big Data, catálogos de productos, redes sociales.   |

---

## 🧩 Reflexión Crítica del Proceso

> 💬 *"Pensar sin tablas nos abrió la mente."*

Durante esta simulación grupal, el ejercicio de diseñar una base de datos sin las estructuras tradicionales relacionales fue un verdadero reto. Aquí algunas conclusiones agrupadas y contrastadas:

### ❗ Lo más difícil de imaginar sin tablas
- 🤯 Romper con la mentalidad de **normalización extrema** fue el mayor obstáculo.
- En SQL, la **redundancia** es considerada un error; en MongoDB, puede ser una **estrategia de rendimiento** válida.
- También costó asumir que los documentos **pueden crecer y divergir**, lo cual exige mayor cuidado y planeación.

### 💡 Lo que nos gustó del enfoque documental
- 📦 La **naturaleza intuitiva** de los documentos: un recurso en la app es un objeto; en MongoDB, también.
- ⚡ La **velocidad de prototipado**: agregar nuevos campos o estructuras es tan simple como modificar el documento.
- ❌ Sin necesidad de ORMs complicados o migraciones estructurales para cada cambio pequeño.

### 🤔 Dudas y reflexiones que surgieron
- 🔁 **¿Cómo garantizar consistencia con datos duplicados?**
  - Para históricos (como recursos pasados): no se tocan.
  - Para datos activos (como información del usuario): se necesita lógica en la app para actualizaciones en cascada.
- 📊 **¿Cómo hacer análisis complejos sin SQL?**
  - El **Aggregation Framework** de MongoDB cubre eso. Es como `GROUP BY`... ¡con esteroides!
- 🔐 **¿Y las transacciones?**
  - MongoDB ≥ 4.0 soporta transacciones ACID multi-documento, pero SQL aún puede ser más robusto en operaciones altamente interdependientes.

---

## 🔍 Recomendación Final

Esta experiencia amplió nuestra perspectiva. Si vienes de SQL, **no asumas que NoSQL es mejor por default**. Evalúa siempre los *trade-offs*:
- ¿Necesitas flexibilidad y escalabilidad horizontal? MongoDB es fuerte.
- ¿Necesitas transacciones complejas y alta consistencia? Tal vez SQL es mejor.

👉 **Se Prueba con datos reales. Se Mide. Se Compara. Se Decide.**

---

## 👨‍💻 Autor

**Daniel Vinasco**

Desarrollado como parte del taller de NO-SQL Documental con MongoDB para "Mis Recursos App"

### Información de Contacto
- **GitHub**: [@DanielSantiagoV](https://github.com/DanielSantiagoV)

---

*Este proyecto cumple con todos los requerimientos especificados en el taller y proporciona una base sólida Documental con MongoDB para "Mis Recursos App".*

---

<p align="center">
  Developed with ❤️ by Estudiante de Base de Datos<br>
  🔥 <b><a href="https://github.com/DanielSantiagoV">Visit my GitHub</a></b> 🚀
</p>