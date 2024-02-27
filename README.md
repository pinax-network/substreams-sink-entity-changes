# Substreams Sink Entity Changes

> [Buf](https://buf.build/) Protobuf codegen & [TypeBox](https://github.com/sinclairzx81/typebox) for [`EntityChanges`](https://github.com/streamingfast/substreams-sink-entity-changes).

## Quickstart

```bash
$ npm install @substreams/sink-entity-changes
```

```typescript
import { typebox, zod } from "@substreams/sink-entity-changes"
```

## Protobuf

```proto
message EntityChanges {
  repeated EntityChange entity_changes = 5;
}

message EntityChange {
  string entity = 1;
  string id = 2;
  Operation operation = 4;
  repeated Field fields = 5;
}
```

### Using [TypeBox](https://github.com/sinclairzx81/typebox) Static Type Resolution

```typescript
import { EntityChanges } from "@substreams/sink-entity-changes/typebox"
...

const emitter = new BlockEmitter(transport, request, registry);

emitter.on("anyMessage", (message: EntityChanges) => {
  for ( const entityChange of message.entityChanges ?? []) {
    console.log(entityChange);
  }
});
```

### Using [Zod](https://github.com/colinhacks/zod) Static Type Resolution

```typescript
import { EntityChanges, getValuesInEntityChange } from "@substreams/sink-entity-changes/zod"
...

const emitter = new BlockEmitter(transport, request, registry);

emitter.on("anyMessage", (message: EntityChanges) => {
  for ( const entityChange of message.entityChanges ?? []) {
    console.log(entityChange);
    const values = getValuesInEntityChange(entityChange);
    const address = values["address"];
  }
});
```

**getValuesInEntityChange**

```typescript
import { getValuesInEntityChange } from "@substreams/sink-entity-changes/zod"
...

for ( const entityChange of message.entityChanges ?? []) {
  const values = getValuesInEntityChange(entityChange);
  const address = values["address"];
}
```

### Using [Buf](https://github.com/bufbuild/protobuf-es) Protobuf codegen

```typescript
import { EntityChanges } from "@substreams/sink-entity-changes/entity_pb"
...

const emitter = new BlockEmitter(transport, request, registry);

// Stream EntityChanges
emitter.on("output", (output: EntityChanges) => {
  for ( const entityChange of output?.entityChanges ?? []) {
    console.log(entityChange);
  }
});
```