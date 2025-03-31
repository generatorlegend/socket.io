# Migration Guide

This guide will help you upgrade your Socket.IO application from previous versions to the latest version. We'll cover breaking changes, new features, and best practices for updating your existing applications.

## Upgrading from 3.x to 4.x

### Breaking Changes

1. **Namespace Middleware Execution**
   - In v3.x, middleware functions were executed upon connection.
   - In v4.x, they are executed before joining a room.

   ```javascript
   // v3.x
   io.use((socket, next) => {
     // executed upon connection
   });

   // v4.x
   io.use((socket, next) => {
     // executed before joining a room
   });
   ```

2. **Socket Middleware Execution**
   - Similar to namespace middleware, socket middleware execution has changed.
   - Now executed before each event instead of once per connection.

3. **Removed `socket.clients()` method**
   - Use `io.in(room).allSockets()` instead.

   ```javascript
   // v3.x
   const clients = await socket.clients();

   // v4.x
   const clients = await io.in(room).allSockets();
   ```

4. **Removed `socket.binary()`**
   - Binary data is now automatically detected.

### New Features

1. **Improved Performance**
   - Reduced CPU usage and memory footprint.

2. **Catch-all Listeners**
   - Listen to all events or all events matching a pattern.

   ```javascript
   socket.onAny((event, ...args) => {
     console.log(`got ${event}`);
   });

   socket.onAnyOutgoing((event, ...args) => {
     console.log(`emitted ${event}`);
   });
   ```

3. **Volatile Events**
   - New `volatile` flag for events that can be dropped.

   ```javascript
   socket.volatile.emit("hello", "might or might not be received");
   ```

### Updating Your Application

1. Review your middleware functions and adjust their logic if necessary.
2. Replace `socket.clients()` calls with `io.in(room).allSockets()`.
3. Remove any `socket.binary()` calls as they are no longer needed.
4. Consider using new features like catch-all listeners and volatile events where appropriate.

## Upgrading from 2.x to 3.x

### Breaking Changes

1. **Removed `socket.io-client` package**
   - Now included in the main `socket.io` package.

2. **Changed default path**
   - Now `/socket.io/` instead of `/socket.io`.

3. **Removed `socket.io-redis`**
   - Use `@socket.io/redis-adapter` instead.

### New Features

1. **TypeScript Support**
   - Built-in TypeScript definitions.

2. **Improved Debugging**
   - More detailed debug output.

### Updating Your Application

1. Update your import statements to use the new package structure.
2. Check your server configuration for the correct path.
3. If using Redis, update to the new adapter package.
4. Take advantage of TypeScript support if applicable to your project.

## Best Practices for Upgrading

1. **Test Thoroughly**: Always test your application extensively after upgrading.
2. **Update Gradually**: If possible, update one component at a time to isolate potential issues.
3. **Review Documentation**: Check the official Socket.IO documentation for detailed information on each version's changes.
4. **Update Dependencies**: Ensure all related dependencies are compatible with the new Socket.IO version.
5. **Backup Your Project**: Always create a backup before starting the upgrade process.

By following this guide, you should be able to smoothly upgrade your Socket.IO application to the latest version, taking advantage of new features while addressing any breaking changes.