# Proyecto Final — Administración de Redes

Automatización del despliegue de **Wiki.js** en servidores Linux usando **Ansible** y **Docker Compose**.

## Descripción

Este proyecto implementa la infraestructura como código para instalar Docker en una instancia EC2 de AWS y desplegar una wiki colaborativa basada en [Wiki.js](https://js.wiki/) con PostgreSQL como base de datos.

## Estructura del repositorio

```
proyecto-final-admin-redes/
├── docker/
│   └── docker-compose.yml    # Servicios Wiki.js + PostgreSQL
├── inventory/
│   ├── hosts.ini             # Inventario para servidor AWS
│   └── local.ini             # Inventario para entorno local
└── playbooks/
    ├── install_docker.yml    # Instalación de Docker y Docker Compose
    └── deploy_wiki.yml       # Despliegue de la wiki
```

## Requisitos previos

- [Ansible](https://docs.ansible.com/) instalado en la máquina de control
- Acceso SSH al servidor destino (clave privada configurada)
- Python 3 en el servidor remoto

## Configuración del inventario

Edita los archivos en `inventory/` según tu entorno:

| Archivo      | Uso                          |
|--------------|------------------------------|
| `hosts.ini`  | Servidor AWS (producción)    |
| `local.ini`  | Servidor local (pruebas)     |

Variables comunes en cada inventario:

- `ansible_user`: usuario SSH (por defecto `ubuntu`)
- `ansible_ssh_private_key_file`: ruta a la clave privada SSH

## Uso

### 1. Instalar Docker en el servidor

```bash
ansible-playbook -i inventory/hosts.ini playbooks/install_docker.yml
```

### 2. Desplegar Wiki.js

```bash
ansible-playbook -i inventory/hosts.ini playbooks/deploy_wiki.yml
```

Para pruebas en entorno local:

```bash
ansible-playbook -i inventory/local.ini playbooks/install_docker.yml
ansible-playbook -i inventory/local.ini playbooks/deploy_wiki.yml
```

## Servicios desplegados

| Servicio   | Imagen                  | Puerto |
|------------|-------------------------|--------|
| Wiki.js    | `requarks/wiki:latest`  | 3000   |
| PostgreSQL | `postgres:15`           | interno|

Tras el despliegue, la wiki queda disponible en `http://<IP-del-servidor>:3000`.

## Autor

Maicol Rojas
