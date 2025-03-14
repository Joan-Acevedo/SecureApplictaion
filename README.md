# Despliegue Seguro de Aplicación Web en AWS

Este proyecto demuestra el despliegue de una **aplicación web segura en AWS**, integrando **Apache HTTP Server**, **Spring Boot** y un cliente **HTML + JavaScript asíncrono**. La solución implementa **cifrado TLS**, autenticación con **contraseñas hasheadas**, y utiliza **certificados Let’s Encrypt** para garantizar la seguridad de extremo a extremo.

---

## Tabla de Contenidos
- [Descripción del Proyecto](#descripción-del-proyecto)
- [Visión General de la Arquitectura](#visión-general-de-la-arquitectura)
- [Requisitos Previos](#requisitos-previos)
- [Instrucciones de Despliegue](#instrucciones-de-despliegue)
- [Implementación de Seguridad](#implementación-de-seguridad)
- [Pruebas y Validación](#pruebas-y-validación)
- [Capturas de Pantalla](#capturas-de-pantalla)
- [Autor](#autor)

---

## Descripción del Proyecto

Esta aplicación web permite servir contenido estático de manera segura y ofrece servicios RESTful. Sus características clave incluyen:

- Comunicación **asíncrona cliente-servidor** mediante HTML + JavaScript.
- Backend **Spring Boot** manejando la lógica de negocio y autenticación de usuarios.
- **Servidor Apache HTTP** sirviendo el cliente y gestionando la terminación TLS.
- **Cifrado TLS** activado tanto en Apache como en el servidor Spring Boot usando **certificados Let’s Encrypt**.
- Autenticación segura con contraseñas **almacenadas como hashes BCrypt**.

---

## Visión General de la Arquitectura

### Componentes y Relación

- **Cliente (HTML + JS)**:
    - Se comunica de forma asíncrona con la API REST.
    - Es servido por Apache a través de HTTPS.

- **Servidor Apache HTTP** (Instancia EC2 #1):
    - Sirve los archivos estáticos (HTML, JS).
    - Maneja la terminación TLS mediante Let’s Encrypt.

- **Aplicación Spring Boot** (Instancia EC2 #2):
    - Expone la API REST del backend.
    - Configurada con cifrado TLS.
    - Maneja la autenticación de usuarios y almacenamiento seguro de contraseñas.

### Diagrama de Arquitectura

![Image](https://github.com/user-attachments/assets/89c365e0-8d3d-445c-9f10-d79bf845f8c8)

---

##  Requisitos Previos

Antes de iniciar el despliegue, asegúrate de tener:

- Cuenta activa en **AWS** con permisos para crear instancias EC2.
- **Instancias EC2** (Ubuntu 20.04 recomendada):
    - Servidor Apache
    - Servidor Spring Boot
- Dominios o subdominios apuntando a ambas instancias (para la emisión de certificados Let’s Encrypt).
- Puertos habilitados en los **Grupos de Seguridad**:
    - Apache: 80 (HTTP), 443 (HTTPS)
    - Spring Boot: 8080 o 443 (según configuración TLS)
- Software necesario instalado:
    - Apache HTTP Server (`sudo apt install apache2`)
    - OpenJDK 17+
    - Certbot para certificados TLS (`sudo apt install certbot`)
    - Spring Boot App (.jar empaquetado)

---

## Instrucciones de Despliegue

### 1. Configuración del Servidor Apache (Instancia EC2 #1)
- Instalar Apache:
  ```bash
  sudo apt update
  sudo apt install apache2

- Copiar el contenido estático (HTML + JS) a `/var/www/html/`
- Habilitar HTTPS con Let’s Encrypt:
  ```bash
  sudo apt install certbot python3-certbot-apache
  sudo certbot --apache

- Desplegar la aplicación Spring Boot:
  ```bash
  java -jar securewebapp-1.0-SNAPSHOT.jar

---

## Implementación de Seguridad

### Certificados TLS
- Se generaron y configuraron **certificados Let’s Encrypt** en ambos servidores:
    - **Servidor Apache**: Certificados obtenidos y configurados automáticamente con `certbot`.
    - **Servidor Spring Boot**: Certificados de Let’s Encrypt exportados y convertidos a **PKCS12** para uso en el keystore de Java.

---

## Pruebas y validación

1. Validación de certificados:
   - Verificado mediante navegador (icono de candado)

2. Autenticación de usuarios:
   - Validada la creación de usuarios y almacenamiento de contraseñas hasheadas.

3. Comunicación segura
   - Se comprobaron todas las comunicaciones con HTTPS y sin advertencias de seguridad.

---

## Capturas de pantalla

A continuación se muestra el funcionamiento de lo implementado:

- Página de inicio

![Image](https://github.com/user-attachments/assets/e1d356d0-4074-4edc-8d06-054df8b4f6ba)

- Página de registro

![Image](https://github.com/user-attachments/assets/0dee78fa-cdde-443a-8159-80fb8dacfab7)

- Confirmación de registro:

![Image](https://github.com/user-attachments/assets/7b5206a6-96fe-44c3-b287-63db87173e2f)

- Login con un usuario valido:

![Image](https://github.com/user-attachments/assets/ae56bf79-8604-4491-96ec-6d0aa96a13a2)

- Login con un usuario invalido:

![Image](https://github.com/user-attachments/assets/99f82a3e-9ff0-40b4-a0d3-0493561400b2)

---

## Autor

**Joan S. Acevedo Aguilar**

Universidad Escuela Colombiana de Ingeniería Julio Garavito

Arquitectura Empresarial 2025-1