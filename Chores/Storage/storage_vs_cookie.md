
## Storage

The Web Storage API provides mechanisms by which **browsers** can securely store ***key/value pairs***, in a much more intuitive fashion than using cookies.

> Web Storage API: setItem, getItem, removeItem, clear, key, length.

### LocalStorage

- Stores data with ***no expiration date***, persists even when the browser is closed and reopened, and gets cleared only through JavaScript, or clearing the Browser cache/Locally Stored Data.
- Data is never transferred to the server.
- Storage limit is larger than a cookie.

### SessionStorage

- The sessionStorage object stores data only for a session, meaning that the data is stored until the browser (or tab) is closed(as long as the browser is open, including page reloads and restores).
- Storage limit is larger than a cookie.
- Data is never transferred to the server.

## Cookie

An HTTP cookie (web cookie, browser cookie) is a ***small piece*** of data that a server sends to the user's web browser. The browser may store it and send it back with the next request to the same server. It remembers **stateful information** for the stateless HTTP protocol.

### Purposes of Cookie

Cookies are mainly used for three purposes:

- Session management
> Logins, shopping carts, game scores, or anything else the server should remember
- Personalization
> User preferences, themes, and other settings
- Tracking
> Recording and analyzing user behavior

> Cookies were once used for general client-side storage. While this was legitimate when they were the only way to store data on the client, it is recommended nowadays to prefer modern storage APIs. Cookies are sent with ***every request***, so they can ***worsen performance*** (especially for mobile data connections):exclamation:.

### Differences from Storage

- Stores data that has to be sent back to the server with subsequent requests. Its expiration varies based on the type and the expiration duration can be set from either server-side or client-side (normally from server-side).
- Cookies are primarily for server-side reading (can also be read on client-side), localStorage and sessionStorage can only be read on client-side.
- Size must be less than 4KB.
- Cookies can be made secure by setting the ***httpOnly*** flag as true for that cookie. This prevents client-side access to that cookie.

> To prevent cross-site scripting (***XSS***) attacks, ***HttpOnly*** cookies are inaccessible to JavaScript's ***Document.cookie*** API(```document.cookie = "newKey=newValue"```); they are only sent to the server. :star: :star:
