# Fake API Fixtures

Static JSON fixtures for experimenting with healthcare facility and shift data. This repository contains data files, not an API server or a database.

## Datasets

| File | Records | Fields |
| --- | --- | --- |
| [facilities.json](facilities.json) | 6 | `id`, `name`, `city`, `priority` |
| [shifts.json](shifts.json) | 22 | `id`, `facilityId`, `userId`, `startTime`, `endTime` |

A shift's `facilityId` refers to a facility's `id`. A null `userId` marks an unassigned shift. Timestamps are ISO 8601 strings in UTC.

## Run locally

Requires Git and Python 3; no packages need installing.

```bash
git clone https://github.com/Lenin-Miranda/fake-api.git
cd fake-api
python3 -m http.server 8000 --bind 127.0.0.1
```

Open [facilities](http://127.0.0.1:8000/facilities.json) or [shifts](http://127.0.0.1:8000/shifts.json). Stop the server with Ctrl+C.

From a page served by the same local server:

```js
const response = await fetch("/shifts.json");
if (!response.ok) throw new Error("Could not load shifts");
const shifts = await response.json();
const available = shifts.filter((shift) => shift.userId === null);
```

## Validate the data

```bash
python3 -m json.tool facilities.json
python3 -m json.tool shifts.json
```

JSON validity does not imply valid business data: shift `18` ends before it starts. This is useful for exercising duration validation. There are no write endpoints, authentication, pagination or automatic CORS configuration.
