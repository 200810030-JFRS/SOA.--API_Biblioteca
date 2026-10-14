# SOA.--API_Biblioteca
# API REST: Biblioteca Virtual (Librería)

##  Descripción de la API
API REST desarrollada bajo los principios de Arquitectura Orientada a Servicios (SOA) para gestionar libros, usuarios y préstamos de una biblioteca virtual. La API permite registrar, consultar, actualizar y eliminar entidades (libros y usuarios), así como gestionar préstamos y devoluciones con control automático de ejemplares disponibles.

## Tecnologías Utilizadas
* **Lenguaje:** Java 21
* **Framework:** Spring Boot 4.1.1
* **Módulos Spring:** Spring Web MVC, Spring Data JPA / Hibernate
* **Base de Datos:** MySQL 8.0
* **Gestor de Dependencias:** Maven Wrapper (`mvnw`)
* **Cliente HTTP:** Postman


## Pasos de Instalación y Ejecución Local

1. Instalar Java 21 y MySQL 8; disponer de acceso local al puerto 3306.
2. Crear en MySQL una base de datos llamada libreria, si todavía no existe.
3. Configurar src/main/resources/application.properties con credenciales locales propias, sin publicarlas.
4. Desde la raíz del proyecto ejecutar .\mvnw.cmd spring-boot:run (Windows).
5. Verificar que aparezca “Started LibreriaApplication” y consultar http://localhost:1600/libros.
   
Parámetros necesarios: spring.datasource.url, spring.datasource.username, spring.datasource.password y server.port=1600. 
Para evitar publicar secretos, use variables de entorno o una configuración local excluida de Git.
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
server.port=1600
spring.jpa.hibernate.ddl-auto=update
Ejemplo de DB_URL para un entorno local de pruebas: jdbc:mysql://localhost:3306/libreria?serverTimezone=UTC. Las opciones de autenticación y TLS deberán ajustarse a la instalación.


Registrar un LibroMétodo / URL: POST http://localhost:1600/librosBody (raw / JSON):{
  "titulo": "El Principito",
  "autor": "Antoine de Saint-Exupery",
  "añoPublicacion": "1943",
  "numeroEjemplares": 5
}
Respuesta: HTTP 200 OK{
  "isbn": 1,
  "titulo": "El Principito",
  "autor": "Antoine de Saint-Exupery",
  "añoPublicacion": "1943",
  "numeroEjemplares": 5
}
2. Registrar un PréstamoMétodo / URL: POST http://localhost:1600/prestamos?usuarioId=1&isbn=1Body: Ninguno (none)Respuesta: HTTP 201 Created{
  "id": 1,
  "fechaPrestamo": "2026-10-14",
  "fechaDevolucion": null,
  "libro": { "isbn": 1, "numeroEjemplares": 4 },
  "usuario": { "id": 1 }
}
3. Registrar una DevoluciónMétodo / URL: PUT http://localhost:1600/prestamos/1/devolverBody: Ninguno (none)Respuesta (Primera vez): HTTP 200 OK
(Actualiza fechaDevolucion a la fecha actual y suma 1 al número de ejemplares)Respuesta (Intento duplicado): HTTP 400 Bad RequestEste libro ya fue devuelto

# Endpoints disponibles
Método	Ruta	Descripción
GET	/libros	Listar libros
GET	/libros/{isbn}	Consultar libro por ID
POST	/libros	Crear libro
PUT	/libros/{isbn}	Actualizar libro
DELETE	/libros/{isbn}	Eliminar libro
GET	/libros/autor/{autor}	Buscar por autor exacto
GET	/libros/titulo/{titulo}	Buscar por título exacto
GET	/usuarios	Listar usuarios
GET	/usuarios/{id}	Consultar usuario
POST	/usuarios	Crear usuario
PUT	/usuarios/{id}	Actualizar usuario
DELETE	/usuarios/{id}	Eliminar usuario
GET	/prestamos	Listar préstamos
GET	/prestamos/{id}	Consultar préstamo
GET	/prestamos/usuario/{usuarioId}	Historial de un usuario
GET	/prestamos/libro/{isbn}	Historial de un libro
POST	/prestamos?usuarioId=1&isbn=1	Registrar préstamo
PUT	/prestamos/{id}/devolver	Registrar devolución


# Complementos: 

## 1. Breve Descripción de cada API

* **API de Libros (`/libros`):** Permite administrar el catálogo de la biblioteca. Mantiene el control del título, autor, año de publicación y disponibilidad de ejemplares en inventario.
* **API de Usuarios (`/usuarios`):** Gestiona la información general del usuario (nombre, correo, teléfono, dirección, rol y estado), requerida para la vinculación con el sistema de préstamos.
* **API de Préstamos (`/prestamos`):** Controla el ciclo de vida del préstamo de libros. Decrementa automáticamente los ejemplares al prestarlos, registra la fecha de inicio, valida que no haya devoluciones duplicadas y reincorpora los ejemplares al catálogo al momento de la devolución.

## 2. Instrucciones para Ejecutar Localmente

1. **Servidor MySQL:**
   * Asegúrate de tener un servicio MySQL activo en `localhost:3306`.
   * Ejecutar en MySQL Workbench: `CREATE DATABASE IF NOT EXISTS libreria;`

2. **Propiedades de la Aplicación:**
   * Verifica la configuración en `src/main/resources/application.properties`:
     ```properties
     spring.datasource.url=jdbc:mysql://localhost:3306/libreria?useSSL=false&serverTimezone=UTC
     spring.datasource.username=root
     spring.datasource.password=tu_contraseña
     server.port=1600
     spring.jpa.hibernate.ddl-auto=update
     spring.jpa.show-sql=true
     ```

3. **Ejecución del proyecto:**
   * Abre la terminal en la raíz del proyecto.
   * Ejecuta en PowerShell: `.\mvnw.cmd spring-boot:run`
   * Espera a visualizar en la consola el mensaje indicando que la aplicación inició en el puerto `1600`.


