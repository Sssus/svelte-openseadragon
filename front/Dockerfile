FROM node:12-alpine AS builder

WORKDIR /app

COPY package.json ./
RUN npm install
COPY . ./
RUN npm run build

EXPOSE 5000
ENV HOST=0.0.0.0

#CMD ["npm", "run", "dev"]
CMD ["npm","start"]