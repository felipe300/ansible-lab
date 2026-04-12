# Laboratorio de Ansible + Docker Compose + LocalStack

[Go back to Readme](../README.md)

Este proyecto implementa un laboratorio local para aprender y probar **Ansible** automatizando infraestructura simulada en AWS mediante **LocalStack**, todo orquestado con **Docker Compose**.

El objetivo es disponer de un entorno 100% reproducible donde puedas practicar playbooks, inventarios, roles y módulos de AWS sin necesidad de una cuenta real ni incurrir en costos.

## Objetivo del Laboratorio

- **Simulación AWS**: Ejecutar servicios de AWS localmente con LocalStack.
- **IaC (Infrastructure as Code)**: Automatizar la creación de recursos (S3, EC2, IAM, VPC) usando Ansible.
- **Ciclo DevOps**: Implementar un flujo completo desde el desarrollo local hasta CI/CD con GitHub Actions.
- **Gestión de Configuración**: Configurar servidores Linux (Docker containers) mediante SSH con Ansible.

## Laboratorio DevOps: LocalStack + Docker + Ansible

Vamos a crear un laboratorio donde:

- Docker Compose levanta:
  - LocalStack (AWS simulado)
  - 2 servidores Linux simulados
- Ansible:
  - Crea infraestructura AWS fake en LocalStack (VPC, subnet, bucket S3)
  - Configura servidores Docker (nginx)
- GitHub Actions valida:
  - Linting del proyecto
  - Levantamiento automático de infraestructura
  - Health checks de LocalStack
  - Validación CI continua en cada `push`/`pull` request

## Tecnologías Utilizadas

| Herramienta    | Propósito                          |
| -------------- | ---------------------------------- |
| Ansible        | Automatización IaC                 |
| Docker         | Virtualización Ligera              |
| Docker Compose | Orquestación de contenedores       |
| LocalStack     | Simulación AWS local               |
| Git / GitHub   | Control de versiones y Repositorio |
| GitHub Actions | CI/CD automatizado                 |

## Arquitectura del proyecto

```bash
ansible-lab/
├── .github/
│   └── workflows/
│       └── ansible-ci.yml
├── docker/
│   ├── Dockerfile
│   └── docker-compose.yml
├── ansible/
│   ├── inventory/
│   │   └── hosts.ini
│   ├── playbooks/
│   │   ├── provision-aws.yml
│   │   └── configure-servers.yml
│   ├── ansible.cfg
│   └── requirements.yml
└── README.md
```

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- Git
- Docker
- Docker Compose
- Ansible (incluyendo la librería `boto3` para módulos AWS)
- Python 3.9+
- AWS CLI (opcional, recomendado)

## Flujo de Trabajo DevOps

- **Levantar entorno**: Iniciar LocalStack y los contenedores de destino.
- **Provisionamiento**: Ejecutar playbooks para crear buckets `S3`, colas o instancias simuladas.
- **Configuración**: Aplicar roles de Ansible sobre los nodos Docker (ej. instalar `Nginx`).
- **Validación**: Confirmar cambios en Git y disparar el CI en GitHub Actions.

## Instalación del Repositorio

**Clonar repositorio**

```bash
git clone https://github.com/felipe300/ansible-lab.git
cd ansible-lab
```

**Variables AWS Dummy**

Crea un archivo `.env` o asegúrate de que las variables estén disponibles en la carpeta `docker/`. LocalStack usará credenciales "dummy":

```bash
# NOTE: This is a basic "EXAMPLE" of your credentials, change them and use new ones

# LocalStack Config
LOCALSTACK_DEBUG=1
AWS_REGION=us-east-1

# Server Config
SERVER_USER=admin
SSH_PORT_1=2221
SSH_PORT_2=2222


# Credentials (Fake for LocalStack)
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
```

**Levantar LocalStack**

La version `localstack:latest` en el archivo `docker-compose` utilizará la versión PRO de LocalStack. Se utilizará la versión `2.3` ó `3.0` para simular algunos sercivios como `s3`, `sqs`, `dynamodb`, o `sns`.

```bash
cd docker
docker compose up -d
```

**Verificar contenedor**

```bash
curl -L http://localhost:4566/_localstack/health | jq | grep "available"
```

Primero, instala las dependencias necesarias:

**Ejecutar Playbook**

```bash
ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
```

Luego, ejecuta el despliegue:

```bash
# Provisionar infraestructura en LocalStack
ansible-playbook -i ansible/inventory/hosts.ini ansible/playbooks/provision-aws.yml

# Configurar servidores locales
ansible-playbook -i ansible/inventory/hosts.ini ansible/playbooks/configure-servers.yml
```

## GitHub Actions: Cómo probar CI

El pipeline se ejecuta automáticamente en:

- `push`
- `pull_request`

**Para disparar el CI**:

Realiza cambios en tu proyecto, depués realiza un `push` a GitHub.

## Autor

**Felipe Gutierrez**

_Laboratorio educativo orientado a la automatización y cultura DevOps_.
