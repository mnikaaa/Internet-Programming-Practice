Week 6 Answers
1. Is DELETE still idempotent?

Yes. An operation is idempotent when sending the same request several times leaves the server in the same final state as sending it once. After the first DELETE /events/2 the event is gone, and after the second one it is still gone. Only the response differs (204 the first time, 404 the second time), but the data on the server is the same.

2. CORS and curl

The browser blocks the response. The API on port 3000 is a different origin from the Vue app on port 5173, and the browser only lets the page read the response if the server sends an Access-Control-Allow-Origin header that allows that origin. curl can read it anyway because CORS is a browser rule, and curl is not a browser, so it does not enforce it.

3. Who creates EventsService?

Nest creates it, using its dependency injection container. It does this once, when the application starts, because EventsController asks for an EventsService in its constructor. Only one instance is created (a singleton) and it is shared, so BookingsService gets the same instance because EventsModule exports it.

4. Refusing "booked": 50

The line forbidNonWhitelisted: true in the ValidationPipe options in src/setup.ts refuses it with a 400 error, together with whitelist: true, which marks properties that are not in the DTO as not allowed. Without it, the field would be silently removed (with only whitelist: true) or, with no whitelist at all, it could reach the service and a client could set booked to any number and fake the seat count.

5. Why is the seats rule in BookingsService?

It is a business rule about the data (capacity and booked), so it belongs in the service, which owns the data. The controller should only translate HTTP into service calls, and the same rule would be needed by anything else that creates bookings. A check only in the Vue form is not enough, because anyone can skip the form and send the request with curl, so the server must always enforce it.