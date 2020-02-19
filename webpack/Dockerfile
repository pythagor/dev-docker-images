FROM node

ENV NODE_ENV=development
ENV PORT=4000

RUN mkdir /workspace

RUN npm config set registry http://registry.npmjs.org/

WORKDIR /workspace

ENTRYPOINT ["tail", "-f", "/dev/null"]
