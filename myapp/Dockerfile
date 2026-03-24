FROM node:18-bullseye-slim

# Set working directory
WORKDIR /app

# Copy package.json
COPY package*.json ./

# Install npm dependencies
RUN npm install

# Update OS and fix vulnerable packages
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --only-upgrade libdb5.3 zlib1g && \
    rm -rf /var/lib/apt/lists/*

# Copy app code
COPY . .

# Expose port
EXPOSE 3000

# Start app
CMD ["npm", "start"]



