# Base image
FROM node:18-bullseye-slim

# Set working directory
WORKDIR /app

# Copy package.json files first (cache npm install)
COPY package*.json ./

# Update OS packages and install dependencies
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y --only-upgrade libdb5.3 zlib1g \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js dependencies
RUN npm install

# Copy application files
COPY . .

# Expose port
EXPOSE 3000

# Start app
CMD ["node", "server.js"]


