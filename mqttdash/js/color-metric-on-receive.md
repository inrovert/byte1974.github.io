Color metric: On Receive
---------------------------------
This handler called when payload received for the metric.
It has nothing to do with visual representation and usually used to handle received data `BEFORE` visual state will be updated.
>`Note`: This handler is executed each time in `CLEAN` mode. This means, that previous call state will be cleaned up (any declared variables and functions from previous call will be purged in subsequent call)!
To pass data between calls and handlers use `event.data`.

### In this handler you can use the following:

####**`app.publish(topic, payload, retained, qos);`**

Publish `payload` (String) to `topic` (String) with specified `retained` flag (Boolean) and `qos` (Integer). Example:

```js
app.publish('led/color', '#ffffff', false, 0);
```

---

####**`app.openUri(uri);`**

Open specified `uri` (String) in an external app. `uri` examples:
- `http://www.example.com/page.html`
- `tel:123`
- `content://contacts/people/`
>Usually you don't want to use it in `On Receive` handler, because `On Receive` executed every time, when payload received.

```js
app.openUri('http://www.example.com');
```
---

####**`event.data`**

(Any type) Custom runtime user data, bound to this particular metric. You can get/set any data, which you need to be preserved between calls.
The data shared between `On Receive`, `On Display`, `On Tap` handlers. So you can set data in `On Receive` and use this data later in `On Display`.
>`Warning`: never pass functions through `event.data`!
The data `NOT` preserved between app launches. Usually, because the data is not assigned by default, you want to start your handler's code with check if data assigned, and then use the data:

In `On Receive` handler:

```js
if (!event.data) {
    event.data = {};
}
event.data['some flag'] = (event.payload == 'something' ? true : false);
```

In `On Display` or in `On Tap` handler:

```js
if (event.data['some flag']) {
    // do something
} else {
    // do something other
}
```

---

####**`event.payload`**

(String) Received raw payload string. You can modify or transform it before it'll be processed and displayed. For example, you can do the following:

---

####**`event.topic`**

(String) Topic, from which payload was received. Use it to distinguish topics in case of using wildcards in your topic (e.g. `#` or `+`). Example:

```js
// if you have configured subscription topic like 'ledlamp/+/set'
if (event.topic == 'ledlamp/kitchen/set') {
    // do something
} else {
    // do something other
}
```
