# fstfy_openapi

A simple plugin for fastify that generates openapi documentation.

## Installation

Just run:

```bash
npm install fstfy_openapi --save
```

## Example

```ts
import fastify from 'fastify';
import schema_validator from 'fluent-json-schema';
const server = fastify();

server.register(require('fstfy_openapi'), {
  openapi : {
    openapi : '3.0.3',
    info : {
      title : 'Fastify Open Api',
      description : 'Fastify Open Api Description',},
      version : '1.0.0'
    },
    servers : [ { url : 'http://127.0.0.1:3000/' } ],
    tags : [ { name : 'service', description : 'Service' } ],
  }
})

const bodyJsonSchema = schema_validator.object()
  .prop('name', schema_validator.string())
  .prop('email', schema_validator.string())

server.route({
  method : 'POST',
  url : '/path',
  schema : {
    body : { $ref: '#request' },
    response : {}
  },
  config: {
    openapi: {
      description: 'Makes a request',
      summary: 'Main route',
      tags: ['service'],
      security: [{ jwtBearer: [] }]
    }
  },
  handler(_, reply) {
    reply.send({ ok: true })
  }
})

server.listen(3000)
```

Once started, the following routes will be available:

- `http://localhost:3000/docs/openapi.yaml`
- `http://localhost:3000/docs/openapi.json`
- `http://localhost:3000/docs` (OpenAPI UI)


