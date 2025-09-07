FROM node:16.19.1@sha256:e97b6d302062583e09be70f430b703637f6ef26c2cbb0d7bfed61e0be9d7a974 AS builder

RUN git clone https://github.com/taigaio/taiga-contrib-oidc-auth.git /app && cd /app && git checkout c623b753047ce9923f3e38614d86e81fc5eb2bb1

WORKDIR /app/front

RUN npm install && npm install gulp

RUN ./node_modules/.bin/gulp build

FROM taigaio/taiga-front:6.8.2@sha256:4ffeac6bca4e56ad280fd033f5c546850723d12dc96bc108da004f9d87be6703 AS final

COPY --from=builder /app/front/dist/ /usr/share/nginx/html/plugins/oidc-auth/

RUN sed -i "s/contribs=()/contribs=('\"plugins\/oidc-auth\/oidc-auth.json\"')/" /docker-entrypoint.d/30_config_env_subst.sh

RUN sed -i '$ s/}$/,\n  "oidcButtonText": "MiData",\n  "oidcButtonImage": "pbs-logo.png",\n  "oidcMountPoint": "\/api\/oidc"\n}/' /usr/share/nginx/html/conf.json.template
