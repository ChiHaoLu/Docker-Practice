## Docker Practice

```
$ docker build -t docker-practice .
$ docker images
>
REPOSITORY                   TAG         IMAGE ID       CREATED          SIZE
docker-practice              latest      16380e9d3875   27 seconds ago   119MB

$ docker run docker-practice
>
Hello World
```

### Reference

- [Docker —— 從入門到實踐](https://philipzheng.gitbook.io/docker_practice/)
- [30分钟Docker入门教程](https://www.youtube.com/watch?v=Ozb9mZg7MVM&ab_channel=GeekHour)
- [Play with Docker](https://www.docker.com/play-with-docker/)
- [Docker Cheatsheet](https://drive.google.com/file/d/1kvkVgVjgGbJ4CVqu68U_OvAxdOoP7ahj/view?usp=sharing)

## Ethereum Client Example

### AA Bundler E2E Test

> [Referemce: safe-modules/4337](https://github.com/safe-global/safe-modules/tree/master/4337#run-end-to-end-tests)

End-to-end tests with the reference bundler implementation are also provided. These automated tests verify that no user operation validation rules are broken by the implementation.

Note: Geth is used as the RPC because the reference bundler implementation relies on Geth-specific tracing APIs.

command
```
$ docker compose up -d
$ npm run test:e2e
```

`./package.json`
```
  "scripts": {
      "test:e2e": "hardhat test --deploy-fixture --network localhost --grep '^E2E - '"
  }
```

`./docker/bundler/Dockerfile`
```
FROM docker.io/library/node:18

RUN git clone https://github.com/eth-infinitism/bundler /src/bundler
WORKDIR /src/bundler
# v0.6.1
RUN git checkout 30dc20da10214415df60a5ee15a6bec0975c9af1

RUN yarn && yarn preprocess
ENTRYPOINT ["yarn", "bundler"]
CMD []
```
