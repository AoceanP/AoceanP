### AoceanP

I build network and security tools, Java backends, and plugins people actually use. Everything below is open source, tested, and documented with diagrams.

## Security tools: five views of one network

Five small tools in four languages, each looking at the same network from a different side. Together they follow one attack from start to finish: someone probes the network, the packets cross the wire, the encryption protecting them is only as good as its configuration, the logon attempts land in the Windows log, and the logs have to stay trustworthy afterwards.

| Project | Language | View | Question it answers |
|---|---|---|---|
| [**portscout**](https://github.com/AoceanP/portscout) | Java 21 | the attacker's view | What ports are open, and what is running behind them? |
| [**wiresight**](https://github.com/AoceanP/wiresight) | C++20 | the network's view | What is actually on the wire, and is someone scanning? |
| [**tlsaudit**](https://github.com/AoceanP/tlsaudit) | Java 21 | the encryption's view | Can the connection itself be trusted? |
| [**logonwatch**](https://github.com/AoceanP/logonwatch) | C# / .NET 10 | the defender's view | Is someone trying to break in, and did they get in? |
| [**tracekeep**](https://github.com/AoceanP/tracekeep) | Python | the evidence's view | Can the logs still be trusted afterwards? |

### The common thread: patient, automated attackers

Automated attackers, including AI-driven ones, are cheap, fast and patient. They try every port, guess passwords without tiring, slow down to stay under alarms, and throw junk at anything that parses input. Each tool answers that in its own layer:

- **portscout** shows the attacker's side, and that a rate limit (`--rate`) makes a scan polite, not invisible.
- **wiresight** catches port scans, host sweeps, and *slow* scans paced under its 10-second alarm (a 15-minute window caught portscout at one port per second). Its own memory is capped, so a SYN flood from forged addresses cannot take down the watcher.
- **logonwatch** catches brute force, password spraying and distributed attacks, plus *slow* guessing and spraying spread over a day. Each alert is tagged with its MITRE ATT&CK technique.
- **tracekeep** ships logs over its own TCP protocol. Both sides prove they hold the key before any data moves, each connection must solve a proof-of-work puzzle that gets harder after every failure, connections are capped per address, and TLS 1.3 is optional.

### They work together

- Run **wiresight** while **portscout** scans: every SYN, SYN-ACK and RST is visible, and so is the alarm. A normal scan sets off **PORT SCAN**; `portscout --rate 1` sets off only **SLOW PORT SCAN**.
- **portscout** finds the open ports; **tlsaudit** checks the encryption on the ones that speak TLS, testing every protocol version and cipher suite one at a time and grading the server from A to F with a reason for every point lost.
- **tlsaudit** audits **tracekeep**'s collector when its TLS mode is on: an **A**, with TLS 1.3 only, three strong suites and a valid certificate.
- **logonwatch** reads the Windows record of everyone who tried to log on; **tracekeep** makes sure nobody can quietly rewrite those logs afterwards.

**409 tests** across the five, covering every boundary and hostile input: truncated packets, injection payloads, fuzzed protocol messages, floods, and deliberately misconfigured TLS servers. Every test and demo runs locally on fake data from the documentation address ranges (RFC 5737). The only outside servers in any example are badssl.com's, which are kept broken on purpose for testing TLS clients.

## Java backend: EasyTravel

[**EasyTravel**](https://github.com/AoceanP/EasyTravel) is flight booking as Spring Boot microservices:
- **Saga:** a booking waits while RabbitMQ carries it through payment, then ends confirmed or failed. Cancelling runs a refund saga back the other way.
- **Infrastructure:** Eureka service discovery, an API gateway, and dead-letter queues.
- **Sign-in:** Keycloak, through a Jakarta Faces web front end.
- **One command:** the whole system starts with `docker compose up`.

## Shipped: RuneLite plugins

Four Java plugins published on the [RuneLite Plugin Hub](https://runelite.net/plugin-hub/) and installed by Old School RuneScape players. Each went through the Hub's code review:

| Plugin | What it does |
|---|---|
| [**Combat & XP Tracker**](https://github.com/AoceanP/combat-xp-tracker) | per-monster damage, max hit and XP/hr, boss kill times, collection-log progress, profit and drop reveals |
| [**Quick Tiles**](https://github.com/AoceanP/quick-tiles) | fast tile marking: paint and erase with hotkeys, a one-click right-click menu, colours, and Ground Markers-compatible import/export |
| [**Grindset**](https://github.com/AoceanP/grindset) | tracks every skilling method on its own card: XP/hr, profit per hour, trip times and time to level |
| [**Immersive Skilling**](https://github.com/AoceanP/immersive-skilling) | visual-only effects: logs pile up, ore glows red-hot and cools, catches fly into a basket |
