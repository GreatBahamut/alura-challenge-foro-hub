# Foro Hub — API REST

Backend de un foro de discusión desarrollado como challenge final del programa **Oracle Next Education (ONE)** de Alura Latam. Permite a usuarios autenticados crear, consultar, actualizar y eliminar tópicos de discusión.

---

## Tecnologías

- **Java 17**
- **Spring Boot 3.5.4**
- **Spring Security + JWT** (Auth0 java-jwt 4.5.0)
- **Spring Data JPA + MySQL**
- **Flyway** — migraciones de base de datos
- **Lombok**
- **SpringDoc OpenAPI** — documentación Swagger UI

---

## Requisitos previos

- Java 17+
- MySQL 8+
- Maven 3.8+

---

## Configuración

El proyecto usa variables de entorno para no exponer credenciales. Antes de correrlo, definí las siguientes:

| Variable | Descripción |
|---|---|
| `DB_URL` | URL de la base de datos, ej: `jdbc:mysql://localhost:3306/foro_hub` |
| `DB_USER` | Usuario de MySQL |
| `DB_PASSWORD` | Contraseña de MySQL |
| `JWT_SECRET` | Clave secreta para firmar los tokens JWT |

Podés definirlas en tu sistema operativo, en un archivo `.env` (con una librería compatible), o directamente en tu IDE como variables de entorno de ejecución.

---

## Cómo correr el proyecto

```bash
# 1. Clonar el repositorio
git clone https://github.com/GreatBahamut/Alura-challenge-foro-hub.git
cd Alura-challenge-foro-hub

# 2. Configurar las variables de entorno (ver sección anterior)

# 3. Compilar y ejecutar
./mvnw spring-boot:run
```

Flyway ejecutará automáticamente las migraciones y dejará la base de datos lista.

---

## Documentación

Con el proyecto corriendo, la documentación interactiva de la API está disponible en:

```
http://localhost:8080/swagger-ui.html
```

---

## Endpoints

### Autenticación

| Método | Ruta | Descripción | Auth requerida |
|---|---|---|---|
| `POST` | `/login` | Obtener token JWT | No |

**Body de ejemplo:**
```json
{
  "login": "usuario@mail.com",
  "contrasena": "tu_contraseña"
}
```

**Respuesta:**
```json
{
  "jwToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### Tópicos

Todos los endpoints de tópicos requieren el token JWT en el header:
```
Authorization: Bearer <token>
```

| Método | Ruta | Descripción |
|---|---|---|
| `GET` | `/topicos` | Listar todos los tópicos (paginado) |
| `GET` | `/topicos/{id}` | Obtener un tópico por ID |
| `POST` | `/topicos` | Crear un nuevo tópico |
| `PUT` | `/topicos/{id}` | Actualizar un tópico |
| `DELETE` | `/topicos/{id}` | Eliminar un tópico |

**Body para crear/actualizar:**
```json
{
  "titulo": "¿Cómo usar Spring Security?",
  "mensaje": "Tengo dudas sobre la configuración de JWT...",
  "autor": "usuario@mail.com",
  "curso": "Spring Boot"
}
```

---

## Reglas de negocio

- No se permiten tópicos con título y mensaje duplicados.
- Todos los campos son obligatorios al crear un tópico.
- La autenticación es requerida para cualquier operación sobre tópicos.
- Las contraseñas se almacenan hasheadas con BCrypt.

---

## Estructura del proyecto

```
src/
└── main/
    ├── java/com/alurachallenges/foro_hub/
    │   ├── controllers/
    │   ├── domain/
    │   ├── infra/
    │   │   └── security/
    │   └── ForoHubApplication.java
    └── resources/
        ├── db/migration/       # Scripts Flyway
        └── application.properties
```

---

## Autor

Desarrollado por [GreatBahamut](https://github.com/GreatBahamut) como parte del programa Oracle Next Education — Alura Latam.
