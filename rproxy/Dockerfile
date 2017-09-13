FROM nginx:alpine

ARG machine_domain

EXPOSE 80

COPY config/servers.conf  /etc/nginx/conf.d/default.conf

#Update Domain
RUN sed -i "s|MACHINE_DOMAIN|${machine_domain}|g" /etc/nginx/conf.d/default.conf

CMD ["nginx", "-g", "daemon off;"]
