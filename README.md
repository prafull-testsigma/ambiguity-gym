# Ambiguity Gym

A deliberately ambiguous app for exercising an agent's "stop and ask" behaviour, and for getting
many test scenarios out of a single learning run.

Every control is ambiguous, slow, flaky or gated on purpose. What the agent asks — and with which
reason — is the thing under test.

Two pages: `index.html` (also served at `/` and `/gym.html`) and `page2.html`.

---

## Page 1 — sections

| # | section | ids | exercises |
| --- | --- | --- | --- |
| 1 | Departure dropdown | `departure` | `data` — three things are called Hyderabad: city, airport, railway station |
| 2 | Four Continue buttons | `continue-a`…`continue-d` | `locator` — identical labels; nothing says which advances |
| 3 | Booking ref / DOB / FFN | `booking-ref` `dob` `ffn` | `input` — values nobody supplied, none may be invented |
| 4 | (no "random world text") | — | `missing` — absent, no near-miss; the dead-end / "Record it" path |
| 5 | Drag handle | `slider` `knob` | recording — drag-only, no click target |
| 6 | Fare class | `fare` | **long choice lists** — twelve near-identical options |
| 7 | Passenger address | `addr1` `addr2` `city` `postcode` `country` `phone` | **sub-actions** — one instruction becomes six actions |
| 8 | Seat map button | `seat-map` | **slow** — does not exist for the first 8 seconds |
| 9 | Hold this fare | `hold-fare` | **flaky** — fails twice with a visible error, succeeds on the third attempt |
| 10 | Terms + Pay now | `terms` `pay` | **preconditions** — Pay is disabled until Terms is ticked |
| 11 | Forty passengers | `pax-01`…`pax-40` | **volume** — long transcripts, scrolling, "go to bottom" |
| 12 | Go to seat selection | `to-page2` | navigation |

**Reset gym** clears every field, re-arms the slow control and resets the flaky counter, so a run
is repeatable without a page reload.

## Page 2 — sections

| section | ids | exercises |
| --- | --- | --- |
| Landed marker | `marker` | a string that exists only here, to prove the agent arrived |
| Ambiguous seats | `seat-A1`…`seat-D6` | identical free seats; "a window seat" needs a person |
| Back / Confirm | `to-page1` `confirm-seats` | a second navigation in one run |

The interaction log on each page appends a line per change, click or drag. It reports what the
**page** received, independently of whether a recorder captured it — useful for telling
"the page never saw the event" apart from "the extension did not capture it".

---

## Recipes — paste these as manual steps

Each recipe is a ready-made test case. Pick by what you want to exercise; they are written to get
the most scenarios out of one run.

### R1 · Question density (4 questions in one run)

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click on Continue
Select the fare class
Fill the passenger address
Select hyderabad
```

Continue is ambiguous, fare has twelve options, address needs values nobody supplied, Hyderabad is
ambiguous with a clear recommendation. Gives you a question with a recommendation, one without, a
long choice list and a free-text question — in a single run.

### R2 · Sub-actions under one step

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Fill the passenger address with 12 Rue Nobel, Apt 4, Lyon, 69003, France, +33 4 72 00 00 00
```

One instruction, six fields. Use it to check that sub-actions appear one by one beneath a single
doing-now line.

### R3 · Retry and failure

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click Hold this fare
Verify the page displays the text Fare held for 20 minutes
```

`hold-fare` fails twice with a visible error before succeeding. Exercises retry behaviour, the
failure card, and what a mid-run failure looks like in the transcript.

### R4 · Preconditions

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click Pay now
```

Pay is disabled until Terms is ticked. The agent should notice the gate rather than report a dead
end — and should say which it is.

### R5 · Slow element

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click Open seat map
```

The button does not exist for 8 seconds. Exercises waiting and what the panel shows while nothing
is happening.

### R6 · Dead end (no recovery path)

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click on missing text
```

There is no such element and no near-miss. Produces reason `missing`, which is what offers
"Record it".

### R7 · Long run (volume, scrolling, long transcript)

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Check in Passenger 01
Check in Passenger 02
Check in Passenger 03
Check in Passenger 04
Check in Passenger 05
Check in Passenger 06
Check in Passenger 07
Check in Passenger 08
Check in Passenger 09
Check in Passenger 10
Check in Passenger 11
Check in Passenger 12
```

Twelve near-identical steps against a scrolling list. Use for long transcripts, the "go to bottom"
affordance, scroll anchoring, and whether expanded step cards survive new rows arriving.

### R8 · Navigation, twice

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click Go to seat selection
Verify the page displays the text Seat selection is open
Select a window seat
Click Back to booking
Verify the page displays the text Ambiguity Gym
```

Two page changes in one run, with a verification either side. "A window seat" is ambiguous — every
free seat is labelled identically.

### R9 · The long one (everything, ~20 steps)

```
Navigate to https://prafull-testsigma.github.io/ambiguity-gym/
Click on Continue
Select the fare class
Fill the passenger address
Select hyderabad
Enter the booking reference, date of birth and frequent flyer number
Click Hold this fare
Accept the fare conditions
Click Pay now
Check in Passenger 01
Check in Passenger 02
Check in Passenger 03
Click Go to seat selection
Verify the page displays the text Seat selection is open
Select a window seat
Click Confirm seats
Click Back to booking
```

One run that touches ambiguity with and without a recommendation, free text, sub-actions, a flaky
control, a gate, volume, and two navigations. Use it when you want maximum coverage per startup.
