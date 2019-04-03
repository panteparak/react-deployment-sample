FROM node:alpine as builder
WORKDIR /frontend
RUN export PATH="$(yarn global bin):$PATH"
RUN yarn global add create-react-app
COPY package.json .
RUN yarn install
COPY . .

RUN yarn build

FROM nginx:alpine
COPY --from=builder /frontend/build /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY deploy/app.template /etc/nginx/conf.d/app.template
WORKDIR /etc/nginx

CMD envsubst "$(env | sed -e 's/=.*//' -e 's/^/\$/g')" < /etc/nginx/conf.d/app.template > /etc/nginx/conf.d/app.conf && exec nginx -g 'daemon off;'
