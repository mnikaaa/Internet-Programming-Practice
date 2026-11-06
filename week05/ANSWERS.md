Week 5 Answers
1. Refreshing on /events/2

In development, the Vite dev server answered the request. It returns index.html for any path that is not a real file, and then Vue Router reads the URL and shows the right page. After deployment, the production server (for example nginx) must do the same: send every path that is not a file to index.html. Otherwise a refresh on /events/2 returns a 404 error.

2. The organiser guard

Yes, a student can still create an event. The guard only runs in the browser, so a student can send the create request directly to the API with curl, Postman or the DevTools console. The server must check the user's role on every create request and reject anyone who is not an organiser, for example with a 403 error.

3. watch with immediate: true instead of onMounted

When the user goes from /events/2 to /events/3, Vue Router reuses the same component instead of creating a new one. With only onMounted, the code would not run again, so the page would keep showing event 2 with the URL of event 3. A watch on the id runs every time the id changes, and immediate: true also makes it run on the first load.

4. storeToRefs instead of const { events } = store

Destructuring with const { events } = store copies the current value once and loses reactivity, so the page would not update when the store changes. storeToRefs turns the store's state into refs, so the page stays reactive.

5. Two requests when booking

If the second request (PATCH) fails, the booking is already saved but the event's seat count was never updated. The data is then inconsistent, and the event can be overbooked. The server can avoid this by doing both steps in one endpoint inside a database transaction, so either both succeed or both are rolled back.