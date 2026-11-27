# Bidirekt

**Know which service your deploy breaks.** Contract testing for microservices, in both directions, without the test environment.

Each service declares, in one YAML file, what it provides and what it reads. A broker you host compares the declarations, and `can-i-deploy` answers before the deploy. When the answer is no, it names the field, the side and the fix. Nothing runs. Works with REST and GraphQL.

```
$ curl -sL bidirekt.com/demo | sh
```

Runs the petstore demo with Docker: a breaking change, `can-i-deploy` catching it, and the fix.

```
petstore_api 2.0.0 cannot be deployed to production

petstore_web (1.0.0, deployed):
  Pet.status — petstore_api doesn't provide it, but petstore_web reads it
    → petstore_api: provide it
    → petstore_web: stop reading it, or handle its absence in the code and mark it optional there
```

## In CI

```
bidirekt publish ./contracts/*.yaml --participant petstore_web --version $SHA
bidirekt can-i-deploy petstore_web --version $SHA --environment production
./deploy.sh
bidirekt record-deployment petstore_web --version $SHA --environment production
```

The exit code of `can-i-deploy` is the gate.

## Repositories

| Repository | What it is | License |
| --- | --- | --- |
| [broker](https://github.com/bidirekt/broker) | Stores the contracts and answers `can-i-deploy`. One Docker image and a Postgres database. | BSL 1.1 |
| [cli](https://github.com/bidirekt/cli) | The `bidirekt` command line client. | Apache 2.0 |
| [typescript-sdk](https://github.com/bidirekt/typescript-sdk) | Writes the contract from TypeScript or JavaScript code. | MIT |
| [php-sdk](https://github.com/bidirekt/php-sdk) | Writes the contract from PHP attributes or docblock tags. | MIT |
| [go-sdk](https://github.com/bidirekt/go-sdk) | Writes the contract from Go code and struct tags. | MIT |
| [skills](https://github.com/bidirekt/skills) | Agent skill that writes and fixes contract files from your code. | Apache 2.0 |
| [petstore-example](https://github.com/bidirekt/petstore-example) | The petstore demo, runnable locally with Docker. | Apache 2.0 |
| [site](https://github.com/bidirekt/site) | bidirekt.com and its docs. | Apache 2.0 |

## Links

- Docs: https://bidirekt.com/docs
- Try it on your project: https://bidirekt.com/docs/start/try
- For agents: https://bidirekt.com/llms.txt
