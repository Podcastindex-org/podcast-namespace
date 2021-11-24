### Social Platform / Protocol

Note: Draft - trying to figure out if the platform/protocol list should be a separate file.

The <social> and <socialInteract> elements both contain a platform and protocol element. This is a list of suitable platforms and protocols

- `platform` (required): This is the platform id. It can be one of the following:
- `protocol` (required): This is the protocol name. It can be one of the following:

| `platform` | `protocol`  |
| ---------- | ----------- |
| castopod   | activitypub |
| mastodon   | activitypub |
| peertube   | activitypub |
|            | xmpp        |
|            | irc         |
| matrix     | matrix      |
| facebook   | facebook    |
| twitter    | twitter     |
| instagram  | instagram   |
| slack      | slack       |
| discord    | discord     |
| 3speak     | hive        |
| peakd      | hive        |


  - `platform` (required): This is the platform id. It can be one of the following:
       - castopod
       - mastodon
       - peertube
       - facebook
       - twitter
       - instagram
       - slack
       - discord
       - 3speak
       - peakd.com
       - …
  - `protocol` (required): This is the protocol name. It can be one of the following:
       - activitypub
       - xmpp
       - irc
       - matrix
       - facebook
       - twitter
       - instagram
       - slack
       - discord
       - hive
       - …