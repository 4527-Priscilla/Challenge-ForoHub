**<h1 align="center"> DESAFÍO FORO HUB </h1>**
<h2 align="center"> ⭐EN FUNCIONAMIENTO ⭐ </h2>
<h3 align="center">Alura Latam ONE - Practicando Spring Framework: Challenge Foro Hub.</h3>
<p align="center">
<img src="assets/Forohub-1.jpg" alt="Imagen título Foro Hub" width="550">
</p>
<p align="center">
<img src="assets/Forohub-2.jpg" alt="Imagen persona usando computador" width="550">
</p>

**Foro Hub** es una solución Backend que replica la lógica de un foro de discusión. Desarrollada con Spring Boot, esta API REST gestiona tópicos, usuarios y seguridad mediante tokens JWT, siguiendo las prácticas de desarrollo y Clean Code aprendidas durante el curso.

## 💻 Tecnologías y Arquitectura
- Java 17 & Spring Boot 3

- Spring Security: Autenticación y Autorización basada en Roles.

- JWT (auth0): Generación y validación de tokens seguros.

- MySQL & Flyway: Persistencia y control de versiones de base de datos.

- Hibernate/JPA: Mapeo objeto-relacional y consultas personalizadas.

## 🤔 Lógica de Negocio Destacada

El corazón de la aplicación reside en su capa de servicios (TopicoService), la cual implementa reglas de negocio críticas:

a) **Validación de Duplicados:** Antes de crear o actualizar, el sistema verifica que no exista ya un tópico con el mismo título y mensaje, evitando contenido redundante.

b) **Protección de Recursos:** Solo el autor de un tópico tiene permisos para realizar actualizaciones o eliminaciones. Si otro usuario lo intenta, el sistema lanza una excepción de AccesoNoAutorizado.

c) **Gestión de Estados:** Los tópicos nacen con el estado NO_RESPONDIDO y pueden transicionar según la actividad del foro.

d) **Seguridad Stateless:** La sesión no se guarda en el servidor; cada petición es validada mediante el Subject y el Issuer del token JWT.

## 👮‍♂️ Control de Acceso con JWT 

La API ForoHub utiliza **JSON Web Tokens (JWT)** para controlar el acceso a todos los endpoints. Solo los usuarios autenticados pueden realizar operaciones CRUD en los tópicos.
Para que la API funcione correctamente, configura las siguientes propiedades en tu **application.properties:**
