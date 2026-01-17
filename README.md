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

## 📊 Modelo de Datos 

La API se basa en un modelo relacional de dos entidades principales, diseñado para garantizar la integridad referencial y la trazabilidad de los tópicos:

a) **Tabla usuarios:** Almacena las credenciales de acceso. Es la entidad "Padre" en la relación.

b) **Tabla topicos:** Almacena el contenido del foro. Cada registro está vinculado obligatoriamente a un usuario mediante la clave foránea usuario_id.

**Relación:** Se implementó una relación One-to-Many (Uno a Muchos) desde Usuario hacia Topico. Esto significa que un usuario puede ser autor de múltiples hilos de discusión, pero cada hilo pertenece a un único autor.

<p align="center">
<img src="assets/Forohub-8.jpg" alt="Imagen de diagrama" width="550">
</p>

## 🤔 Lógica de Negocio Destacada

El corazón de la aplicación reside en su capa de servicios (TopicoService), la cual implementa reglas de negocio críticas:

a) **Validación de Duplicados:** Antes de crear o actualizar, el sistema verifica que no exista ya un tópico con el mismo título y mensaje, evitando contenido redundante.

b) **Protección de Recursos:** Solo el autor de un tópico tiene permisos para realizar actualizaciones o eliminaciones. Si otro usuario lo intenta, el sistema lanza una excepción de AccesoNoAutorizado.

c) **Gestión de Estados:** Los tópicos nacen con el estado NO_RESPONDIDO y pueden transicionar según la actividad del foro.

d) **Seguridad Stateless:** La sesión no se guarda en el servidor; cada petición es validada mediante el Subject y el Issuer del token JWT.

## 🏠 Estructura de Base de Datos

El esquema se autogenera mediante Flyway al arrancar la app:

**usuarios:** Almacena credenciales seguras (BCrypt).

**topicos:** Almacena la conversación vinculada al usuario.

## 👮‍♂️ Control de Acceso con JWT 

La API ForoHub utiliza **JSON Web Tokens (JWT)** para controlar el acceso a todos los endpoints. Solo los usuarios autenticados pueden realizar operaciones CRUD en los tópicos.
Para que la API funcione correctamente, configura las siguientes propiedades en tu **application.properties:**
<p align="center">
<img src="assets/Forohub-3.jpg" alt="Imagen ejemplo clave reemplazo de MySQL" width="550">
</p>

Recuerda que en caso de que tengas registrada una contraseña en MySQL, debes reemplazarla en "spring.datasource.password=TU_CONTRASENA_DE_MYSQL_AQUI!". En la siguiente imagen muestro el lugar en donde se almacena tu contraseña MySQL.

<p align="center">
<img src="assets/Forohub-4.jpg" alt="Imagen ejemplo clave de MySQL" width="550">
</p>

Una vez hecho esto, podemos utilizar Postman App para realizar pruebas.

## ℹ️ Guía de Uso con Postman

Para probar la API en Postman o Insomnia, sigue estos pasos:

1) **Login:** Envía un POST a /login con el usuario admin123.

2) **Obtener Token:** Copia el valor del campo token de la respuesta JSON.

3) **Configurar Auth:** En las peticiones de tópicos (POST, PUT, DELETE), ve a la pestaña Auth, selecciona Bearer Token y pega tu token.

## 📑 Pruebas en Postman
### Configuración inicial

1. Abre Postman
2. Crea una nueva colección llamada "ForoHub API"
3. Configura la variable de entorno:
   - **Variable:** `base_url`
   - **Valor inicial:** `http://localhost:8080`
     
Ahora puedes ingresar:

- **Recurso:** Auth    |    **Método:** POST    |    **Endpoint:** /login    |    **Acción:** Inicia sesión y genera JWT.
- **Recurso:** Foro    |    **Método:** GET    |    **Endpoint:** /topicos    |    **Acción:** Lista 10 tópicos por fecha.
- **Recurso:** Foro    |    **Método:** POST    |    **Endpoint:** /topicos    |    **Acción:** Crea un tópico (Requiere Auth).
- **Recurso:** Foro    |    **Método:** PUT    |    **Endpoint:** /topicos/{id}    |    **Acción:** Edita (Solo el autor).
- **Recurso:** Foro    |    **Método:** DELETE    |    **Endpoint:** /topicos/{id}    |    **Acción:** Elimina (Solo el autor).

Ejemplo de /login; Haz clic en Send. La respuesta devolverá un Token.
<p align="center">
<img src="assets/Forohub-5.jpg" alt="Imagen ejemplo /login en Postman" width="550">
</p>

Copia todo el texto largo (sin las comillas) en Auth, Auth Type Bearer Token.
Ahora que tienes el token, se puede intentar crear un tópico o ver la lista protegida.

<p align="center">
<img src="assets/Forohub-6.jpg" alt="Imagen ejemplo Token en Postman" width="550">
</p>

Crea otra pestaña en Postman para un método GET o POST (por ejemplo, POST http://localhost:8080/topicos).

<p align="center">
<img src="assets/Forohub-7.jpg" alt="Imagen ejemplo /topicos en Postman" width="550">
</p>

👀 **OJO!** Si no quieres estar pegando el token cada vez que expire, puedes hacer esto:

- En Postman, haz clic en tu Colección de peticiones.

- Ve a la pestaña Authorization de la colección.

- Configura ahí el Bearer Token.

- En cada petición individual, asegúrate de que en la pestaña Authorization diga "Inherit auth from parent".

*¡Así, al cambiar el token en un solo lugar, se actualiza en todas las peticiones!*

<p align="center">---------------------------------------------------------------------------------------------</p>

### Autor

Proyecto desarrollado como parte del programa **Oracle Next Education en Alura LATAM.**

