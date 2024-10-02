![[image.png]]


![[image 1.png]]


```bash
# pull official base image  
FROM node:16.10.0-alpine  
  
# set working directory  
WORKDIR /app  
  
# add `/app/node_modules/.bin` to $PATH  
ENV PATH /app/node_modules/.bin:$PATH  
  
# install app dependencies  
COPY package.json ./  
COPY yarn.lock ./  
RUN yarn install --silent  
  
ENV REACT_APP_BASE_URL "localhost:8088"  
  
# add app  
COPY . ./  
  
# start app  
CMD ["yarn", "start"]
```

```bash
FROM artifactory.lebedev.com:6087/source/zulu:17  
COPY src/main/resources/certs /certs/  
WORKDIR /app  
COPY --from=BUILD /app/build/libs/*.jar /app/app.jar  
ENTRYPOINT ["sh", "-c", "java ${JAVA_OPTS} -jar app.jar"]
```


```
docker run -p 8080:80 nginx
```
