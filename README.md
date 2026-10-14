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

Requisitos previos:Tener instalado Java 21 JDK.Servidor MySQL 8 activo en el puerto 3306.Crear la Base de Datos:
Abre MySQL Workbench o la terminal de MySQL y ejecuta:CREATE DATABASE IF NOT EXISTS libreria;
Configurar variables de entorno o archivo de propiedades:
Modifica o genera el archivo src/main/resources/application.properties asegurándote de no exponer datos sensibles.Ejecutar la aplicación:
En Windows (PowerShell):.\mvnw.cmd spring-boot:run
En Linux/macOS:./mvnw spring-boot:run
Verificación:
Confirma que en la consola aparezca Started LibreriaApplication y prueba la ruta base http://localhost:1600/libros. Variables de Entorno NecesariasPara proteger credenciales sensibles en entornos públicos, utiliza variables de entorno en lugar de contraseñas explícitas en el código:DB_URL: jdbc:mysql://localhost:3306/libreria?serverTimezone=UTCDB_USER: Nombre del usuario de MySQL (ej. root)DB_PASSWORD: Contraseña del usuario de MySQLEjemplo de application.properties:spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
server.port=1600
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
Listado de Endpoints Libros (/libros)MétodoRutaDescripciónGET/librosListar todos los librosGET/libros/{isbn}Consultar libro por ID (ISBN)POST/librosCrear un libroPUT/libros/{isbn}Actualizar datos de un libroDELETE/libros/{isbn}Eliminar un libroGET/libros/autor/{autor}Buscar libros por autor exactoGET/libros/titulo/{titulo}Buscar libros por título exacto Usuarios (/usuarios)MétodoRutaDescripciónGET/usuariosListar todos los usuariosGET/usuarios/{id}Consultar usuario por IDPOST/usuariosCrear un usuarioPUT/usuarios/{id}Actualizar datos de un usuarioDELETE/usuarios/{id}Eliminar un usuario Préstamos (/prestamos)MétodoRutaDescripciónGET/prestamosListar todos los préstamosGET/prestamos/{id}Consultar préstamo por IDGET/prestamos/usuario/{usuarioId}Historial de préstamos por usuarioGET/prestamos/libro/{isbn}Historial de préstamos por libroPOST/prestamos?usuarioId=1&isbn=1Registrar un nuevo préstamoPUT/prestamos/{id}/devolverRegistrar devolución de préstamo Ejemplos de Peticiones y Respuestas1. Registrar un LibroMétodo / URL: POST http://localhost:1600/librosBody (raw / JSON):{
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

# PARTE 2: Documento Técnico de Entrega (Reporte)

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


