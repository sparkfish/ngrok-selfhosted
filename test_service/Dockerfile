FROM node:14

WORKDIR /code

COPY [".", "/code/"]

RUN npm i -g json-server

EXPOSE 4000

CMD ["json-server", "--watch", "db.json", "--port", "4000", "--host", "0.0.0.0"]
