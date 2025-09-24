Despliegue de la Aplicación con Nginx y Docker (Windows / PowerShell)

Este proyecto muestra cómo desplegar una aplicación web usando Nginx dentro de un contenedor Docker en Windows, asegurando que la aplicación quede accesible desde el navegador.

Requisitos previos

Tener instalado Docker Desktop
 en Windows.

Tener tu aplicación web lista (React, Angular, Vue o HTML/CSS/JS).

Tener PowerShell para ejecutar los comandos.

Estructura recomendada del proyecto
dockerPractica1/
├── app/           # Archivos compilados de la aplicación
├── nginx.conf     # Configuración personalizada de Nginx
├── Dockerfile     # Para construir la imagen Docker
└── README.md      # Documentación

Paso 1: Copiar tu aplicación a la carpeta app

Abrir PowerShell y moverse a la carpeta del proyecto Docker:

cd "C:\Users\Oliver\Desktop\dockerPractica1"


Crear la carpeta app:

mkdir app


Copiar los archivos de tu aplicación:

Copy-Item -Path "C:\Users\Oliver\Documents\Cine2025\*" -Destination .\app -Recurse

Paso 2: Crear la configuración de Nginx

Crear el archivo nginx.conf:

New-Item -Path . -Name "nginx.conf" -ItemType "File"


Abrir nginx.conf y pegar:

server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}

Paso 3: Crear el Dockerfile

Crear el archivo Dockerfile:

New-Item -Path . -Name "Dockerfile" -ItemType "File"


Abrir Dockerfile y pegar:

# Imagen base de Nginx
FROM nginx:alpine

# Eliminar configuración por defecto
RUN rm /etc/nginx/conf.d/default.conf

# Copiar configuración personalizada
COPY nginx.conf /etc/nginx/conf.d

# Copiar la aplicación compilada
COPY app/ /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

Paso 4: Construir la imagen Docker
docker build -t dockerpractica1-nginx .

Paso 5: Limpiar contenedores antiguos (opcional, para evitar conflictos)

Si ya habías ejecutado contenedores con esta imagen, primero deténlos y elimínalos:

docker ps -a --filter ancestor=dockerpractica1-nginx -q | ForEach-Object { docker stop $_; docker rm $_ }

Paso 6: Ejecutar el contenedor
docker run -d -p 8081:80 --name mi-aplicacion dockerpractica1-nginx


-d → ejecutar en segundo plano.

-p 8081:80 → puerto host:puerto contenedor.

--name mi-aplicacion → nombre del contenedor.

dockerpractica1-nginx → nombre de la imagen.

Abrir el navegador:

http://localhost:8081
