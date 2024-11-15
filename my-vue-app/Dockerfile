# Use the official Node.js image as a base
FROM node:lts-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and yarn.lock
COPY package.json yarn.lock ./

# Install dependencies using Yarn
RUN yarn install

# Copy the rest of the application code
COPY . .

# Expose port 5173 (default port for Vite)
EXPOSE 5173

# Command to run the app using Vite
CMD ["yarn", "dev", "--host"]