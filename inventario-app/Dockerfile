# =========================================================
# ETAPA 1: Construcción (Node.js)
# =========================================================
# Usamos Node.js en su versión miniatura (alpine) para compilar
FROM node:20-alpine AS build

# Creamos la carpeta de trabajo
WORKDIR /app

# Copiamos primero SOLO los archivos de dependencias
COPY package*.json ./

# Instalamos los paquetes de Angular (npm install)
RUN npm install

# Copiamos el resto de tu código fuente (html, ts, css)
COPY . .

# Compilamos el proyecto para producción
# Esto generará los archivos finales dentro de la carpeta 'dist'
RUN npm run build

# =========================================================
# ETAPA 2: Servidor Web (Nginx)
# =========================================================
# Nginx es un servidor web ultra rápido y ligero
FROM nginx:alpine

# Copiamos los archivos estáticos ya compilados desde la etapa 'build'
# OJO: En Angular 17+, la ruta por defecto suele añadir '/browser' al final
COPY --from=build /app/dist/inventario-app/browser /usr/share/nginx/html

# Exponemos el puerto 80 (el estándar para páginas web)
EXPOSE 80

# Encendemos el servidor Nginx
CMD ["nginx", "-g", "daemon off;"]