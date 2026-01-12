# Kafka Local con Docker Desktop

Este `docker-compose.yml` configura un servidor Kafka local para desarrollo y pruebas.

## Requisitos

- Docker Desktop instalado y ejecutándose
- Docker Compose (incluido en Docker Desktop)

## Uso

### 1. Iniciar Kafka

```bash
docker compose up -d
```

Esto iniciará Kafka en segundo plano.

### 2. Verificar que está funcionando

```bash
docker compose ps
```

Deberías ver el contenedor `kafka-broker` con estado "Up".

### 3. Ver los logs

```bash
docker compose logs -f kafka
```

### 4. Crear un topic de prueba

```bash
docker exec -it kafka-broker /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic
```

### 5. Listar topics

```bash
docker exec -it kafka-broker /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

### 6. Detener Kafka

```bash
docker compose down
```

### 7. Detener y eliminar datos (volúmenes)

```bash
docker compose down -v
```

## Configuración para el Notebook

Si quieres usar este Kafka local en lugar del remoto, actualiza la **Celda 2** del notebook:

```python
KAFKA_BOOTSTRAP_SERVERS = ["localhost:9092"]
KAFKA_TOPIC = "bigdata_ut3"
USE_SASL_SSL = False
KAFKA_SECURITY_CONFIG = {
    "security_protocol": "PLAINTEXT"
}
```

## Comandos útiles

### Producir mensajes de prueba

```bash
docker exec -it kafka-broker /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic
```

### Consumir mensajes

```bash
docker exec -it kafka-broker /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```

### Ver información del topic

```bash
docker exec -it kafka-broker /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic bigdata_ut3
```

## Configuración

- **Puerto**: 9092 (accesible desde `localhost:9092`)
- **Particiones por defecto**: 3
- **Retención de logs**: 7 días
- **Modo**: KRaft (sin necesidad de Zookeeper)

## Solución de problemas

### El puerto 9092 ya está en uso

Si tienes otro Kafka corriendo, detén el otro servicio o cambia el puerto en `docker-compose.yml`:

```yaml
ports:
  - "9093:9092"  # Cambiar el puerto externo
```

### El contenedor no inicia

Verifica los logs:

```bash
docker compose logs kafka
```

### Limpiar todo y empezar de nuevo

```bash
docker compose down -v
docker compose up -d
```
