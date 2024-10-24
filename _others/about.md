---
title: About others
author: Tao He
date: 2022-02-04
category: Jekyll
layout: post
---

This is an about page for "others" in the collections.

## Ejecutar proyecto

```bash
bundle exec jekyll serve
```

# Verificación de Instalación: Docker y Git

Este documento detalla los pasos para verificar si Docker y Git están instalados en tu sistema y cómo obtener las versiones correspondientes.

## 1. Verificar si Docker está instalado

Para verificar si Docker está instalado y cuál es su versión, ejecuta el siguiente comando en la terminal o línea de comandos:

```bash
docker --version
```

Si Docker está instalado correctamente, verás un resultado similar a:

```bash
Docker version 20.10.12, build e91ed57
```

Si no está instalado, recibirás un mensaje de error indicando que el comando `docker` no se encuentra.

### Instalar Docker

Si no tienes Docker instalado, puedes seguir la [guía oficial de instalación de Docker](https://docs.docker.com/get-docker/) para tu sistema operativo.

---

## 2. Verificar si Git está instalado

Para verificar si Git está instalado y cuál es su versión, ejecuta el siguiente comando en la terminal o línea de comandos:

```bash
git --version
```

Si Git está instalado correctamente, verás un resultado similar a:

```bash
git version 2.34.1
```

Si no está instalado, recibirás un mensaje de error indicando que el comando `git` no se encuentra.

### Instalar Git

Si no tienes Git instalado, puedes seguir la [guía oficial de instalación de Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) para tu sistema operativo.

---

## 3. Comprobaciones adicionales

Una vez confirmada la instalación de Docker y Git, puedes ejecutar los siguientes comandos adicionales para verificar su correcto funcionamiento:

- **Probar Docker con un contenedor de prueba**:
  
  ```bash
  docker run hello-world
  ```

  Esto descargará y ejecutará una imagen de prueba para verificar si Docker está funcionando correctamente.

- **Probar Git con la configuración global**:
  
  Verifica la configuración de Git ejecutando:

  ```bash
  git config --list
  ```

  Si Git está configurado, verás una lista de opciones de configuración.

---

## 4. Solución de problemas

Si tienes problemas al instalar o verificar Docker o Git, revisa las guías oficiales o consulta la documentación específica para tu sistema operativo:

- [Documentación oficial de Docker](https://docs.docker.com/)
- [Documentación oficial de Git](https://git-scm.com/doc)
