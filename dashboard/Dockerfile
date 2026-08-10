FROM node:20-alpine AS builder

WORKDIR /app

# Copy the ROOT package.json one level up so vite.config.ts can resolve
# new URL('../package.json', import.meta.url) → /package.json
COPY package.json /package.json

# Copy dashboard package files and install dependencies
COPY dashboard/package*.json ./
RUN npm ci --include=dev

# Copy dashboard source code
COPY dashboard/ .

# Build the app
RUN npm run build

# Production stage with nginx
FROM nginx:alpine

# Copy built files
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy nginx config
COPY dashboard/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
