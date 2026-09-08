# Ambiguity Gym

A deliberately ambiguous page for exercising an agent's "stop and ask" behaviour.

Every control here is ambiguous on purpose. What the agent asks — and with which reason — is the
thing under test.

| section | exercises | why it is ambiguous |
| --- | --- | --- |
| Departure dropdown | `data` | three things are called Hyderabad: city, airport, railway station |
| Four Continue buttons | `locator` | identical labels; nothing says which one advances |
| Booking ref / DOB / FFN | `input` | values nobody supplied, and none may be invented |
| (no "random world text") | `missing` | absent, with no near-miss to tempt a wrong click |
| Drag handle | recording | drag-only, no click target — a person has to demonstrate it |

The interaction log at the bottom appends a line per change, click or drag. It reports what the
**page** received, which is independent of whether a recorder captured it — useful for telling
"the page never saw the event" apart from "the extension did not capture it".

Served at `/` and at `/gym.html`; the two files are identical.
