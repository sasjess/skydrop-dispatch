# Feature requests

Build these as separate pull requests to your team's fork. Each one says who usually takes it, which files it touches and how to tell it's done.

| | Feature | Usually taken by | Files |
| --- | --- | --- | --- |
| F1 | Show the whole fleet | Lists | `FleetList.jsx` |
| F2 | A proper drone card | Props | `DroneCard.jsx` (+ a new component if you like) |
| F3 | Filter the fleet | State + Events, after F1 | `FleetList.jsx` |
| F4 | New delivery requests | Events | `RequestForm.jsx`, `addRequest` in `App.jsx` |
| F5 | Assign a drone | State | `RequestCard.jsx`, `RequestBoard.jsx`, `assignDrone` in `App.jsx` |
| F6 | The full request board | Lists | `RequestBoard.jsx`, `RequestCard.jsx`, `markDelivered` in `App.jsx` |
| F7 | A page for every drone | Routes | `main.jsx`, `App.jsx`, `Header.jsx`, `DronePage.jsx`, `DroneCard.jsx` |
| F8 | Keep the data after a refresh | State | `App.jsx` |

Teams without a Routes expert: the Props expert takes F7 after F2. The State expert takes F8 after F5. F5 and F6 both touch `RequestCard.jsx` and `RequestBoard.jsx`, so agree who edits what first.

---

## F1 · Show the whole fleet

Only the first drone shows. Show all eight.

**Done when:**
- [X] Every drone in `initialDrones` has a card.
- [X] Each card has a `key` that is unique and never changes (no warning in the browser console).
- [X] The heading says how many drones there are: "Fleet (8)".

*Hint:* `RequestBoard.jsx` already does this for pending requests. Look at how it uses `map`.

---

## F2 · A proper drone card

A card only shows the id and the name. Dispatchers need to see the rest at a glance.

**Done when:**
- [ ] The card shows the model and how much it can carry ("Carries up to 5 kg").
- [ ] It shows the drone's status using the existing `StatusPill` component.
- [ ] It shows the battery as a number and a bar whose width is the battery percentage.
- [ ] A drone under 25% battery shows a "Low battery" warning. Others don't.
- [ ] Everything comes from the `drone` prop. Nothing is typed in by hand.

*Hint:* a bar's width can come from a value: `style={{ width: `${drone.battery}%` }}`.

---

## F3 · Filter the fleet

*Build after F1 is merged.*

**Done when:**
- [ ] Above the fleet there are four tabs: All, Available, Delivering, Charging.
- [ ] Each tab shows its count, for example "Available (5)".
- [ ] Clicking a tab shows only the drones with that status. All shows everything.
- [ ] The selected tab looks selected.
- [ ] If a tab has no drones, it says "No drones here right now."

*Hint:* the selected tab is state. The list you show can be worked out from the drones and the selected tab. It doesn't need its own state.

---

## F4 · New delivery requests

The form looks right but does nothing, and pressing the button reloads the page.

**Done when:**
- [ ] The inputs are controlled: one `form` object in state, one `handleChange` that uses each input's `name`.
- [ ] Pressing **Add request** doesn't reload the page.
- [ ] If anything is empty, or the weight isn't between 0.1 and 8 kg, an error message says what's wrong and nothing is added.
- [ ] A valid request appears in the Pending column with status "pending", no drone, a new id (REQ-105, REQ-106, …) and the current time.
- [ ] The form clears after a successful add.
- [ ] The area is stored as its id (`col-05`), not its label.

*Hint:* `addRequest` in `App.jsx` is already passed to the form as `onCreate`. The form collects; `App` adds to the list.

---

## F5 · Assign a drone

Pending requests sit there forever. Dispatchers need to send a drone.

**Done when:**
- [ ] Each pending request has a dropdown of drones that can take it: status "available", battery at least 30%, and `maxLoadKg` at least the parcel's weight.
- [ ] An **Assign** button sends the chosen drone. It is disabled until a drone is chosen.
- [ ] After assigning, the request's status is "assigned" and it remembers the drone's id. The drone's status is "delivering".
- [ ] Both changes happen in `assignDrone` in `App.jsx`, using copies, never by changing the old arrays or objects.
- [ ] A request with no suitable drone says "No drone can carry this right now."

*Hint:* `requests.map((request) => request.id === requestId ? { ...request, status: "assigned", droneId } : request)`.

---

## F6 · The full request board

Only Pending exists. Add the rest of the journey.

**Done when:**
- [ ] The board has three columns: Pending, Assigned and Delivered, each with its count.
- [ ] Assigned cards show which drone has the parcel (by name, not id).
- [ ] Assigned cards have a **Mark delivered** button.
- [ ] Marking delivered moves the request to Delivered. The drone goes back to "available" with 15% less battery (never below 0).

*Hint:* the three columns are the same thing with a different status. Could one small component draw all three?

---

## F7 · A page for every drone

**Done when:**
- [ ] The app uses React Router: `/` shows the dashboard, `/drones/SD-03` shows that drone's page, and any other address shows `NotFound`.
- [ ] Each drone card's name links to its page.
- [ ] The drone page reads the id from the URL with `useParams`, shows the drone's details and lists the requests it has carried.
- [ ] `/drones/SD-99` says there's no drone with that id and links back.
- [ ] The header has a **Dashboard** link that looks active when you're on the dashboard.

*Hint:* wrap `<App />` in `<BrowserRouter>` in `main.jsx`. Both pages need `drones` and `requests`, so pass them as props from `App`.

---

## F8 · Keep the data after a refresh

Refresh the page and every new request and assignment is gone. Dispatchers can't work like that.

**Done when:**
- [ ] A `useEffect` saves `drones` to `localStorage` whenever they change, and another saves `requests`. Each lists exactly what it saves in its dependency array.
- [ ] On start, the app loads the saved drones and requests, or the starting data if nothing is saved yet.
- [ ] The browser tab's title shows the pending count, "SkyDrop · 2 pending", and updates whenever it changes. This is a third `useEffect`.
- [ ] No effect runs in a loop. Check the console: nothing logs over and over.

*Hint:* `localStorage` only stores text: `JSON.stringify` to save, `JSON.parse` to load. Loading can happen in `useState(() => …)`, which runs once on the first render. To start again while testing, clear the site's storage in DevTools → Application.

---

## Stretch, if your team finishes

- **S1 · Live counts in the header:** available drones, pending requests and delivered today. Work them out from the state; don't store them.
- **S2 · Search the fleet:** a search box that finds drones by name, ignoring capitals and extra spaces.
- **S3 · Reset button:** a "Reset demo data" button that clears the saved data and puts the starting drones and requests back.
