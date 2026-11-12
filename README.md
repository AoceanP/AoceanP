### AoceanP

I build network and security tools, Java backends, and plugins people actually use. Everything below is open source, tested, and documented with diagrams.

## Security tools: five views of one network

One planned series in four languages. Together the tools follow an attack from start to finish, and each one is built to hold up against patient, automated attackers.

| Project | Language | View | Question it answers |
|---|---|---|---|
| [**portscout**](https://github.com/AoceanP/portscout) | Java 21 | the attacker's view | What ports are open, and what is running behind them? |
| [**wiresight**](https://github.com/AoceanP/wiresight) | C++20 | the network's view | What is on the wire, and is someone scanning, even slowly? |
| [**tlsaudit**](https://github.com/AoceanP/tlsaudit) | Java 21 | the encryption's view | Can the connection itself be trusted? |
| [**logonwatch**](https://github.com/AoceanP/logonwatch) | C# / .NET 10 | the defender's view | Is someone trying to break in, and did they get in? |
| [**tracekeep**](https://github.com/AoceanP/tracekeep) | Python | the evidence's view | Can the logs still be trusted afterwards? |

The tools check each other: wiresight catches portscout's scans, even at one port per second, and tlsaudit gives tracekeep's encrypted collector an A. 409 tests across the five, all running locally on fake data.

## Also

- [**EasyTravel**](https://github.com/AoceanP/EasyTravel) (Java, Spring Boot): flight booking as microservices, with a payment saga over RabbitMQ, an API gateway, Keycloak sign-in, and `docker compose up` to run it all.
- [**blendsolve**](https://github.com/AoceanP/blendsolve) (Python, PyTorch): recovers facial blendshape weights from noisy synthetic face captures, about 5x more accurate than least squares.
- **RuneLite plugins**, four on the [Plugin Hub](https://runelite.net/plugin-hub/), used by Old School RuneScape players and each through the Hub's code review: [Combat & XP Tracker](https://github.com/AoceanP/combat-xp-tracker), [Quick Tiles](https://github.com/AoceanP/quick-tiles), [Grindset](https://github.com/AoceanP/grindset) and [Immersive Skilling](https://github.com/AoceanP/immersive-skilling).
