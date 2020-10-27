FROM bcit/openshift-openresty:latest

ENV KEYCLOAK_ENDPOINT      "https://keycloak.example.com/auth"
ENV KEYCLOAK_REALM         "master"
ENV KEYCLOAK_CLIENT_ID     "nginx"
ENV KEYCLOAK_CLIENT_SECRET "00000000-0000-0000-0000-000000000000"

LABEL maintainer="jesse@weisner.ca, chriswood.ca@gmail.com"
LABEL build_id="1603824004"

USER root

RUN opm install \
        bungle/lua-resty-session \
        zmartzone/lua-resty-openidc

ADD https://github.com/jweisner/lua-resty-keycloak/raw/nginx-exit/lib/resty/keycloak.lua /usr/local/openresty/site/lualib/resty/keycloak.lua

RUN chmod 0664 /usr/local/openresty/site/lualib/resty/keycloak.lua

COPY openid-callback.conf /usr/local/openresty/nginx/conf
COPY openid-auth.conf     /usr/local/openresty/nginx/conf
COPY openid-authz.conf    /usr/local/openresty/nginx/conf
COPY default.conf         /usr/local/openresty/nginx/conf.d
COPY 00-openresty.conf    /usr/local/openresty/nginx/conf.d

RUN chown -R :root /usr/local/openresty \
 && chmod -R g+rw  /usr/local/openresty

COPY 50-copy-config.sh /docker-entrypoint.d/

USER nginx