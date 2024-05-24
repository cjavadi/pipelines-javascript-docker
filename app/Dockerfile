# Stage 1: Build (Node.js environment)
FROM node:18-slim AS builder

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy the rest of the application code
COPY . .

# Stage 2: Runtime (Minimal image)
FROM node:18-alpine AS runtime

# Create a non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Copy the application code and node_modules from the builder stage
COPY --from=builder /app .

# Ensure the application runs with the non-root user
USER appuser

EXPOSE 8080
CMD [ "node", "server.js" ]
