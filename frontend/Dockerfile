# Stage 1: Build
FROM node:18-alpine3.18 AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
ENV VITE_API_URL="https://enzotec.com.ng"
RUN npm run build

# Stage 2: Serve with NGINX
FROM nginx:alpine AS production

# Remove default NGINX index page
RUN rm -rf /usr/share/nginx/html/*

WORKDIR /app
COPY --from=builder /app .
# Copy built assets from builder stage
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy custom NGINX config (optional)
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

