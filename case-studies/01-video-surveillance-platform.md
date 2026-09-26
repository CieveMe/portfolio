# Case study 1 — Video surveillance & vehicle-device management platform

**Role**: full-stack developer / delivery owner · **Period**: 2026 · **Team**: me + client-side testers

## The problem

The client ran a fleet-and-site operation with cameras and vehicle-mounted devices from several vendors. They needed one console for live preview, playback, vehicle location, RFID events and department-level permissions — and they needed it delivered as a runnable system, not a demo, because the device side could only be validated on site.

Constraints that shaped everything:

- **Mixed device population**: GB28181 devices and sub-platforms, RTSP cameras, plus non-video data (RFID readers, sensors).
- **Browser playback had to seek**, which rules out naive "pipe the stream through and hope" designs.
- **Data isolation by department** was a hard requirement — one department must not see another's devices through any API, including playback.
- **On-site validation only**: features that could not be exercised on real hardware had to be marked as such.

## What I built

**Streaming layer**

- GB28181 / RTSP ingest into ZLMediaKit, with H.264/H.265 handling and shared-stream reuse so that N viewers of the same channel cost one upstream stream.
- Browser playback with proper seek, served through authenticated HTTP Range requests instead of full downloads.
- New-NVR queueing with rate limiting and a cooldown window, plus faulted-channel recovery, to get rid of the SIP 400/503 errors that appeared under concurrent viewing.
- Validated with 5 simultaneous 720p streams before handover.

**Data layer**

- GA/T 1400 image and structured-data ingestion, wired into the same device model as the video channels.
- MQTT integration for RFID and telemetry devices: QoS 1, persistent sessions, topic-level permissions, and idempotent processing so reconnects do not create duplicate records.
- Vehicle position, RFID events and department tree in one data model, with a department-scoped data scope enforced across list, detail, playback and control APIs.

**Delivery layer**

- Docker deployment with versioned backups and a rehearsed rollback path.
- A project knowledge base and deployment documentation, then a source handover package.
- **139 backend tests** and **6 frontend regression checks** green on the delivered version.

## Result

- Live preview, playback with seek, vehicle map, RFID data and department permissions all working on the client's own hardware.
- Concurrency problems that previously produced SIP 400/503 stopped appearing after the queueing/cooldown change.
- The client could redeploy and roll back without me: deployment and rollback were rehearsed, not described.

## What I would do differently

The NVR queueing and cooldown logic should have been in the first design instead of a fix after the concurrency testing. Next time I would write down the maximum concurrent viewers as an explicit requirement at kickoff, because "how many people watch at once" turned out to be the single most load-bearing number in the whole system.

## Notes on evidence

Client name, contracts, endpoints and customer data are omitted on purpose. The figures above are from the delivery records; anything that could only be confirmed on site was verified there, and features that were not exercised on real hardware are not claimed here.
