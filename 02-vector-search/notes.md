# 🧠 Apuntes sobre Vector Search Engine y RAG

## ✅ Naturaleza de la data y configuración del motor vectorial

Antes de construir el índice vectorial, es **clave entender la estructura y propósito de los datos**. Esto permite:

- Elegir el chunking adecuado (tamaño, solapamiento).
- Decidir qué entra en el embedding y qué va como metadata.
- Optimizar la recuperación semántica.

---

## ✅ Embeddings vs Metadata

- **Texto principal (respuesta, contenido explicativo, etc.)** → debe ir al **embedding**, porque es lo que se usará para calcular similitud semántica.
- **Campos como `course`, `section`, `autor`, `fecha`, etc.** → van en **metadata**, ya que no aportan directamente al significado semántico, pero **pueden usarse como filtros** o para enriquecer la respuesta.

📌 Algunos motores como **Weaviate**, **Qdrant** o **Pinecone** permiten hacer búsquedas híbridas (vector + filtro por metadata).

> **Embedding = lo que quiero que el modelo "entienda"**  
> **Metadata = lo que quiero que el sistema "organice o filtre"**

---

## ✅ Flujo de funcionamiento RAG

### 🔹 1. Offline (Indexación)
- Preprocesamiento del contenido (limpieza, chunking).
- Generación de **embeddings** con un modelo (ej. `text-embedding-3-small`, `all-MiniLM`, etc.).
- Almacenamiento de cada chunk con su embedding y metadata en el **vector store**.

### 🔹 2. Online (Consulta)
- La **pregunta del usuario** se convierte en un embedding.
- Se hace una **búsqueda semántica** en el índice vectorial.
- Se recuperan los `k` documentos más similares.

### 🔹 3. Generación
- Los documentos recuperados se pasan como **contexto** al modelo generativo (ej. GPT-4).
- El modelo genera una respuesta **basada en ese contexto**.

---

## ✅ Consideraciones Finales

- Es importante saber cómo es la naturaleza de nuestra data para configurar correctamente el motor vectorial.
- Decidir qué entra en el embedding y qué va en metadata es **clave** para la eficiencia del sistema.
- El texto (respuesta) debe ir al embedding.
- Campos como `course` y `section` van en metadata porque no se busca directamente por ellos, sino que sirven como guía o filtro.