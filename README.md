# Estructura del proyecto
cypress/
├── e2e/
│   └── user-service/
│       └── get-user.cy.ts
├── fixtures/
│   └── user-service/
│       └── get-user.data.ts
└── support/
    ├── e2e.ts
    └── grpc/
        ├── assertions/
        │   └── user-response.assertions.ts
        ├── clients/
        │   └── user-service.client.ts
        ├── constants/
        │   ├── grpc-endpoints.constants.ts
        │   ├── grpc-metadata.constants.ts
        │   └── grpc-status.constants.ts
        ├── tasks/
        │   └── grpc.tasks.ts
        └── types/
            └── grpc-task.types.ts

cypress.config.ts
src/
└── generated/
    └── user_pb.ts[2:50 PM]
  
cypress/e2e/user-service/get-user.cy.ts: contiene la prueba del happy path de GetUser; invoca la tarea gRPC y comprueba el resultado.
cypress/fixtures/user-service/get-user.data.ts: guarda los datos del caso de prueba: request, Bearer token, metadata indicador y respuesta esperada.
cypress/support/e2e.ts: archivo de soporte general de Cypress, cargado antes de los specs.
assertions/user-response.assertions.ts: agrupa las comprobaciones del status gRPC y de los campos de la respuesta.
clients/user-service.client.ts: configura y crea el cliente gRPC para UserService.
constants/grpc-endpoints.constants.ts: define la dirección del servicio gRPC local.
constants/grpc-metadata.constants.ts: centraliza los nombres de las metadata usadas en la llamada.
constants/grpc-status.constants.ts: define y organiza los códigos y nombres de estado gRPC.
tasks/grpc.tasks.ts: implementa la tarea que realiza la llamada gRPC desde Node y entrega su resultado a Cypress.
types/grpc-task.types.ts: define los tipos TypeScript para el request, metadata, respuesta y resultado de la tarea.
cypress.config.ts: configura Cypress, indica dónde encontrar los specs y registra las tareas de Node.
src/generated/user_pb.ts: código generado a partir del proto; contiene las definiciones TypeScript del servicio y sus mensajes.
[2:52 PM]

#Usa estas librerias

@bufbuild/protobuf (^2.15.0) — runtime de mensajes protobuf generados
@connectrpc/connect (^2.2.0) — cliente Connect/gRPC
@connectrpc/connect-node (^2.2.0) — transporte gRPC sobre Node (HTTP/2)

# Para subir el grpc server
npm --prefix grpcServer start

# Para ejecutar la prueba (es solo de happy path de un servicio gRPC con autenticacion, metadata)
npm run cy:run -- --spec cypress/e2e/user-service/get-user.cy.ts

# Con este comando puedes probar el gRPC "users.UserService/GetUser" (tiene un Bearer Token, un metadata que se llama indicador):

grpcurl \
  -plaintext \
  -import-path ./proto-files \
  -proto user.proto \
  -H "authorization: Bearer token123" \
  -H "indicador: yes" \
  -d '{"id":"123"}' \
  0.0.0.0:50051 \
  users.UserService/GetUser

# La respuesta esperada es:
{
  "id": "123",
  "name": "Alice Mocked via WireMock Cloud",
  "email": "alice-cloud@example.com"
}
