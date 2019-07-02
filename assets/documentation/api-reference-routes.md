![nodegate](../images/logo-documentation.png)

---

# [Documentation](README.md) > [API reference](api-reference.md) > Routes

Routes are objects defining the behavior of nodegate for a specific path and method.

_Properties_

| Argument   | Type       | Description                                                                                                               |
| :--------- | :--------- | :------------------------------------------------------------------------------------------------------------------------ |
| `method`   | `string`   | **Required.** Method of the route (`get`, `post`, `patch`, …).                                                            |
| `path`     | `string`   | **Required.** Path of the route, the path can be written on the Express way, for example: `/user/:id`                     |
| `pipeline` | `array`    | **Required.** List of the modifiers to apply to the container.                                                            |
| `onError`  | `function` | Callback to execute in case of error. This callback must return a container object. See [Error handling](#error-handling) |

The pipelines are the list of modifiers to execute **synchronously** to modify the container.

Each modifier of the pipeline will be called with two arguments:
 - The container, with the update of the previous modifier
 - The original requests received by nodegate. (The Express request).

## Error handling

To allow a better management of the errors, a callback property named `onError` can be set.
This callback will receive a [PipelineError](api-reference-pipelineerror.md) argument, this error
contains:

 - The error message,
 - If applicable, the last response received by **nodegate**,
 - The container.

**This callback must return a container object** used by the route executor to send the response to
the client.

In case of error, the statusCode will be set to 500. If a response exists on the error, the status
code will be the same as the response statusCode.

_Examples_

To answer a specific body with the error:

```js
gateway.route({
  method: 'get',
  path: '/gateway-route',
  pipeline: [...],
  onError: (error) => {
    return {
      ...error.container,
      body: {
        reason: 'An unknow error occured.',
      }
    }:
  };
});
```

To define a specific error code:

```js
gateway.route({
  method: 'get',
  path: '/gateway-route',
  pipeline: [...],
  onError: (error) => {
    return {
      ...error.container,
      statusCode: 503,
    }:
  };
});
```

---

**[Next: Modifiers](api-reference-modifiers.md)**
