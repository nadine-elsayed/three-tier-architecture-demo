FROM node:18-slim

# Install python and build tools (required for some npm packages)
RUN apt-get update && apt-get install -y \
    python3 \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*

ENV INSTANA_AUTO_PROFILE true
EXPOSE 8080

WORKDIR /opt/server

COPY package.json /opt/server/

# Use --quiet to make the logs cleaner and avoid timeout errors
RUN npm install --quiet

COPY server.js /opt/server/

CMD ["node", "server.js"]