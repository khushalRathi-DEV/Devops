FROM node

WORKDIR /app/

COPY . .

RUN npm install

CMD ["node", "index.js"]


#docker run -d -p 27017:27017 --network my-net --name mongodb mongo