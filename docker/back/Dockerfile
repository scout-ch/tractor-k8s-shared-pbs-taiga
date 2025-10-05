FROM taigaio/taiga-back:6.8.3@sha256:92d0cdd62b4fc20eab206347ba9bdd20f4a39b42227bd96bb9323a5f3f58a916 AS base

FROM alpine/git@sha256:e67f8df098804a09c8cd54510b076a44a475a0ef9e25371fc8601f9cd81ae092 AS cloner

RUN git clone https://github.com/taigaio/taiga-contrib-oidc-auth.git && cd taiga-contrib-oidc-auth && git checkout c623b753047ce9923f3e38614d86e81fc5eb2bb1

FROM base AS builder

COPY ./config-extension.py /tmp/config-extension.py

RUN cat /tmp/config-extension.py >> /taiga-back/settings/config.py

COPY --from=cloner /git/taiga-contrib-oidc-auth /taiga-contrib-oidc-auth

RUN cd /taiga-contrib-oidc-auth/back && python setup.py sdist && ls -lA

FROM base AS final

COPY ./urls.py /taiga-back/settings/urls.py

COPY --from=builder /taiga-back/settings/config.py /taiga-back/settings/config.py

COPY --from=builder /taiga-contrib-oidc-auth/back/dist/taiga-contrib-oidc-auth-0.3.tar.gz /tmp/taiga-contrib-oidc-auth.tar.gz

RUN pip install /tmp/taiga-contrib-oidc-auth.tar.gz && rm /tmp/taiga-contrib-oidc-auth.tar.gz