# Contexto Del Proyecto

## Propósito

API de cartas con arquitectura hexagonal y dos adaptadores de infraestructura independientes: Spring Boot y Quarkus.

## Módulos

- `core/`: dominio, casos de uso y puertos. No depende de Spring, Quarkus, JPA ni Scryfall.
- `infra-spring-boot/`: adaptadores HTTP, persistencia PostgreSQL y cliente Scryfall para Spring Boot.
- `infra-quarkus/`: adaptadores equivalentes para Quarkus.
- `docs/api/card/`: colección Bruno con las peticiones de la API.

El comportamiento compartido se define en `core`. Cada infraestructura compone el servicio de cartas conectando sus adaptadores a los puertos del core.

## Requisitos

- Java 25.
- Maven 3.9 o Maven Wrapper.
- Docker o Podman.
- PostgreSQL.

La base de datos local usa `card_db`, usuario `db_user`, contraseña `db_password` y puerto `5432`, según `docker-compose.yaml`.

## Comandos

Desde la raíz:

```bash
docker compose up -d card-database
mvn clean install
mvn -pl infra-spring-boot spring-boot:run
mvn -pl infra-quarkus quarkus:dev
```

Spring Boot usa el puerto `8002` y Quarkus el puerto `8001`.

## API

Ambas infraestructuras exponen los mismos endpoints:

- `POST /api/cards` con `{ "card_name": "lightning bolt" }`.
- `GET /api/cards`.

La creación consulta Scryfall, normaliza el nombre y persiste la carta.

## Reglas de mantenimiento

- Mantener `core` libre de dependencias de framework e infraestructura.
- Implementar cualquier comportamiento común en puertos, casos de uso o dominio antes de duplicarlo en los adaptadores.
- Mantener equivalencia funcional entre Spring Boot y Quarkus.
- Actualizar la colección Bruno si cambian los endpoints.
- Ejecutar `mvn clean install` tras modificar más de un módulo.
- Mantener documentación y comentarios en español neutro; usar identificadores Java y rutas de API en inglés.
