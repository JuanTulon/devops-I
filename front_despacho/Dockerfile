# --- Etapa 1: Construcción (Build) ---
FROM node:20-alpine AS build
WORKDIR /app

# Copiar archivos de dependencias para aprovechar la caché de Docker
COPY package*.json ./
RUN npm ci

# Copiar el resto del código y compilar
COPY . .
RUN npm run build

# --- Etapa 2: Producción (Servidor Web Seguro) ---
FROM nginxinc/nginx-unprivileged:1.25-alpine
# Copiamos los archivos estáticos generados en la etapa anterior
# Nota: Cambia "/dist" por "/build" si tu proyecto usa Create React App en vez de Vite
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]