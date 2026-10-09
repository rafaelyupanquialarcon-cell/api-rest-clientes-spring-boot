# API REST de Clientes en Spring Boot

## 1. Descripción del proyecto

El proyecto consiste en documentar una API REST para gestionar clientes mediante operaciones de registro, consulta, actualización y eliminación. También permitirá buscar clientes por DNI o por coincidencia de nombre.

La finalidad es mantener la información organizada y facilitar su consulta mediante solicitudes HTTP.

## 2. Requisitos técnicos de arquitectura

### 2.1. Tecnologías

- **Lenguaje:** Java 21 o Java 25.
- **Framework:** Spring Boot 3.x.
- **Spring Web:** permite crear los endpoints REST.
- **Spring Data JPA:** facilita la interacción con la base de datos.
- **Spring Validation:** permite validar los datos recibidos.
- **Base de datos:** H2 en memoria.
- **Pruebas:** JUnit 5, Mockito y Spring Boot Test.
- **Documentación de API:** Swagger / OpenAPI.

### 2.2. Configuración de la base de datos

Se utilizará una base de datos H2 en memoria para almacenar temporalmente los registros de clientes durante las pruebas locales.

Configuración prevista:

- URL: `jdbc:h2:mem:clientesdb`
- Consola web de H2: habilitada para inspeccionar las tablas durante las pruebas.
- Persistencia: los datos son temporales y normalmente se pierden al cerrar la aplicación.

### 2.3. Manejo de excepciones

Se utilizará `@RestControllerAdvice` para centralizar el tratamiento de errores.

Las respuestas de error seguirán el formato `ProblemDetail`, basado en RFC 7807, cuando corresponda.

Se contemplan los siguientes casos:

- Datos de entrada inválidos.
- DNI duplicado.
- Cliente no encontrado.
- Errores relacionados con las solicitudes HTTP.

### 2.4. Validaciones principales

- El DNI es obligatorio y debe contener exactamente ocho dígitos.
- El nombre es obligatorio.
- El correo electrónico debe tener un formato válido.
- El DNI debe ser único.
- El cliente debe existir para poder actualizarlo o eliminarlo.

## 3. Historias de usuario y criterios de aceptación

### HU-01: Gestión CRUD de clientes

**Como** analista de datos,  
**quiero** registrar, consultar, actualizar y eliminar clientes,  
**para** mantener actualizada la base de datos operativa.

#### Escenario 1: Creación exitosa

- **Given:** se envía una solicitud `POST /api/v1/clientes` con un DNI único, nombre y correo válidos.
- **When:** la API procesa el registro.
- **Then:** responde con HTTP `201 Created`, incluye el encabezado `Location` y devuelve el cliente creado con su ID.

#### Escenario 2: DNI duplicado

- **Given:** ya existe un cliente con el DNI `12345678`.
- **When:** se intenta registrar otro cliente con el mismo DNI.
- **Then:** la API responde con HTTP `409 Conflict` e informa del conflicto de unicidad.

### HU-02: Búsqueda y filtrado por DNI o nombre

**Como** operador del sistema,  
**quiero** buscar clientes por DNI o por coincidencia de nombre,  
**para** encontrar rápidamente su información.

#### Escenario 1: Búsqueda exacta por DNI

- **Given:** existe un cliente con el DNI `12345678`.
- **When:** se realiza la solicitud `GET /api/v1/clientes?dni=12345678`.
- **Then:** la API responde con HTTP `200 OK` y devuelve los datos del cliente encontrado.

#### Escenario 2: Búsqueda parcial por nombre

- **Given:** existen clientes cuyos nombres contienen la palabra Carlos.
- **When:** se realiza la solicitud `GET /api/v1/clientes?nombre=Carlos`.
- **Then:** la API responde con HTTP `200 OK` y devuelve los clientes coincidentes sin distinguir entre mayúsculas y minúsculas.

## 4. Contrato de la API REST

| Método | Endpoint | Descripción | Respuestas esperadas |
|---|---|---|---|
| POST | `/api/v1/clientes` | Crear cliente | 201 Created / 400 Bad Request / 409 Conflict |
| GET | `/api/v1/clientes` | Listar o buscar clientes | 200 OK |
| GET | `/api/v1/clientes/{id}` | Obtener cliente por ID | 200 OK / 404 Not Found |
| PUT | `/api/v1/clientes/{id}` | Actualizar cliente | 200 OK / 400 Bad Request / 404 Not Found |
| DELETE | `/api/v1/clientes/{id}` | Eliminar cliente | 204 No Content / 404 Not Found |

### Parámetros y datos

- **POST:** recibe los datos mediante `ClienteRequestDTO`.
- **GET /api/v1/clientes:** admite los parámetros opcionales `dni` y `nombre`.
- **GET por ID:** recibe el identificador del cliente en la ruta.
- **PUT:** recibe el ID y los datos actualizados mediante `ClienteRequestDTO`.
- **DELETE:** recibe el ID del cliente que se desea eliminar.

## 5. Lista de cotejo: Definition of Ready (DoR)

La Definition of Ready permite verificar si una historia de usuario está preparada para iniciar su desarrollo.

- [ ] La historia de usuario cumple el principio INVEST: es independiente, negociable, valiosa, estimable, pequeña y testeable.
- [ ] El contrato de la API, incluidos DTO, rutas y respuestas HTTP, está definido.
- [ ] Las reglas de validación del DNI y del correo electrónico están documentadas.
- [ ] La estimación en Story Points ha sido asignada por el equipo técnico.

## 6. Lista de cotejo: Definition of Done (DoD)

La Definition of Done permite comprobar si el trabajo cumple las condiciones necesarias para considerarse terminado.

- [ ] El código funciona y la base de datos H2 en memoria está configurada.
- [ ] Las pruebas unitarias con JUnit 5 y Mockito se ejecutan correctamente.
- [ ] Las pruebas de integración con `@SpringBootTest` se ejecutan correctamente.
- [ ] El Pull Request es pequeño, revisado y aprobado por un desarrollador responsable.
- [ ] El análisis estático cumple los controles de calidad y seguridad establecidos.
- [ ] La cobertura de código alcanza el mínimo aprobado para el proyecto.
- [ ] La documentación interactiva de la API está disponible mediante Swagger / OpenAPI.

## 7. Glosario técnico

- **API REST:** interfaz que permite intercambiar información mediante solicitudes HTTP.
- **Commit:** registro de un conjunto de cambios en el historial de Git.
- **Conflict:** incompatibilidad entre cambios realizados en un mismo proyecto.
- **HTTP:** protocolo utilizado para la comunicación entre clientes y servidores web.
- **Merge:** integración de cambios procedentes de diferentes ramas.
- **Remote:** repositorio remoto con el que se sincroniza el repositorio local.
- **Repository:** espacio donde se almacenan los archivos y el historial de cambios del proyecto.
- **Resolve:** acción de solucionar un conflicto de cambios.
- **Revision:** versión registrada de un proyecto.
- **SHA-1:** función hash utilizada históricamente por Git para identificar objetos y commits.
- **SSH:** protocolo para establecer conexiones seguras entre equipos.
- **Timestamp:** marca que indica la fecha y hora de un evento.
- **Version control:** sistema que permite registrar, comparar y recuperar cambios realizados en los archivos.

## 8. Conclusión

La documentación permite definir los requisitos técnicos, las operaciones principales de la API y las condiciones que deben cumplir las historias de usuario. También ayuda a establecer criterios para comprobar la calidad del trabajo antes de considerarlo terminado.

## 9. Recomendaciones

- Mantener actualizada la documentación cuando cambien los endpoints.
- Validar los datos recibidos antes de guardarlos.
- Ejecutar pruebas unitarias y de integración.
- Revisar los cambios antes de integrarlos al repositorio principal.
- No considerar las tareas del DoD completadas hasta contar con evidencias de su cumplimiento.
