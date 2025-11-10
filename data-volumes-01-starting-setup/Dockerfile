# Use a lightweight Node.js base image
FROM node:18-alpine

# Set the work directory for the application
WORKDIR /app

# Install dependencies separately to leverage Docker layer caching
COPY package*.json ./
RUN npm install --production && npm cache clean --force

# Copy the rest of the application source
COPY . .

# Ensure directories used for runtime data exist and are owned by the non-root user
RUN mkdir -p temp feedback \
    && chown -R node:node temp feedback

# Expose the HTTP port used by the Express app
EXPOSE 80

# Switch to the non-root user for better security
USER node

# Start the HTTP server
CMD ["node", "server.js"]
