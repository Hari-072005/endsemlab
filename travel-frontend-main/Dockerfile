## Multi-stage Dockerfile to build Vite app and serve with nginx
# Build stage
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
# Optional: keep default nginx config (serves index.html) — good for SPA
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
