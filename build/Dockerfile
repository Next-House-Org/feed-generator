# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

# Copy source code
COPY . .

# Compile TypeScript
RUN yarn build

# Stage 2: Runtime
FROM node:20-alpine AS runner
WORKDIR /app

# Install important tools (curl, vim, git)
RUN apk update && \
    apk add --no-cache curl vim git

# Copy only runtime files from the builder stage
COPY --from=builder /app ./

# Expose the port (adjust if different in config.ts)
EXPOSE 3000

ENV NODE_ENV=production

# Run the service
CMD ["node", "dist/index.js"]

