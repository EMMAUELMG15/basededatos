# Práctica 1: "Modelo Entidad Relación"

**Instituto Politécnico Nacional**  
**Escuela Superior de Cómputo (ESCOM)**  
**Unidad de Aprendizaje:** Bases de Datos  
**Programa Académico:** Ingeniería en Sistemas Computacionales (Plan 2020)  

---

## Datos del Alumno
- **Nombre completo:** Emmanuel Morales Gonzalez
- **Carrera:** Ingeniería en Sistemas Computacionales
- **Grupo:** [Ingresa tu grupo aquí, ej. 3CV1]
- **Boleta:** [Ingresa tu boleta aquí]

---

## Índice de Contenidos y Entregables

1. **[Ejercicio 1: Control de versiones con Git y GitHub](docs/investigacion-ejercicio1.md)**
   - Investigación teórica: control de versiones, Git vs. GitHub, glosario y flujos de trabajo.
   - Evidencia de ramas y fusión: [Captura Pull Request Merged](evidencias/git/pull-request-merged.png).
   - Evidencia del árbol de confirmaciones: [Captura git log --graph](evidencias/git/git-log-graph.png).

2. **[Ejercicio 2: El sistema gestor en un contenedor: Docker](docs/investigacion-ejercicio2.md)**
   - Configuración orquestada de PostgreSQL: [compose.yaml](entorno/compose.yaml).
   - Evidencia de conexión y creación de la BD `practica1`: [Captura Conexión](evidencias/docker/conexion-db.png).
   - Evidencia de prueba de persistencia con volúmenes: [Captura Persistencia](evidencias/docker/persistencia.png).

3. **[Ejercicio 3: Investigación: Fundamentos de Bases de Datos (PDF)](docs/investigacion-bases-de-datos.pdf)**
   - Documento formal de la Unidad Temática I (conceptos, ciclo de vida, modelos, arquitectura de 3 niveles y normas APA 7).

4. **[Ejercicio 4: Estado del Arte: Literatura Científica Arbitrada (PDF)](docs/estado-del-arte.pdf)**
   - Fichas analíticas de tres artículos científicos indexados (SIGMOD / ACM) con DOI y análisis comparativo.

5. **[Ejercicio 5: Caso de Estudio y Modelo Entidad-Relación (PDF)](docs/caso-de-estudio.pdf)**
   - Problemática, requerimientos funcionales y entrevista simulada de la boutique de ropa.
   - [Diagrama Entidad-Relación (PNG)](modelo/diagrama-er.png).

---

## Instrucciones para levantar el entorno local

Para ejecutar el gestor de base de datos PostgreSQL 16 localmente:

```bash
# Levantar el servicio en segundo plano
docker compose -f entorno/compose.yaml up -d

# Conectarse a la consola interactiva psql
docker exec -it pg-practica1 psql -U postgres