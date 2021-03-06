# Disrupt (Docker Swarm Service Updater)

[![GitHub release](https://img.shields.io/github/v/release/BlameButton/disrupt?include_prereleases)](https://github.com/BlameButton/disrupt/releases)
[![GitHub](https://img.shields.io/github/license/BlameButton/disrupt)](https://github.com/BlameButton/disrupt/blob/master/LICENSE)
[![GitHub contributors](https://img.shields.io/github/contributors/blamebutton/disrupt?style=flat-square)](https://github.com/BlameButton/disrupt/graphs/contributors)

[![Build Status](https://img.shields.io/travis/com/BlameButton/disrupt/master?style=flat-square)](https://travis-ci.com/BlameButton/disrupt)
[![Docker Pulls](https://img.shields.io/docker/pulls/blamebutton/disrupt?style=flat-square)](https://hub.docker.com/r/blamebutton/disrupt)
[![Discord Widget](https://img.shields.io/discord/556492964050763817?style=flat-square)](https://discord.gg/tDf2yBg)

## Usage

```yaml
version: '3'

services:
  whoami:
    image: jwilder/whoami
    ports:
      - 8080:8000

  disrupt:
    image: blamebutton/disrupt
    volumes:
      # Mount the Docker socket, this is required for interacting with the Docker API.
      - /var/run/docker.sock:/var/run/docker.sock
    deploy:
      placement:
        constraints:
          # Disrupt needs permission to update services, only managers are allowed to do that.
          - node.role == manager
```

For more examples, check out the [examples folder](/.examples)

**Note**: Docker Swarm does not support running locally built images, so neither does Disrupt.

## Configuration

Configuration of Disrupt is done through environment variables. This is done to make the deployment of Disrupt
environment-agnostic.

Below is a list of environment variables available to be configured.

| Name | Type | Options |
| - | - | - |
| UPDATE_DELAY | Integer | default = 300 |
| NOTIFICATION_URL | String | Any [Apprise](https://github.com/caronc/apprise#popular-notification-services) compatible URL |

## Notifications

Disrupt uses the [Apprise](https://github.com/caronc/apprise) notification library for Python.
Check out their documentation for more advanced usages.

Here is a small list of notification providers that Apprise supports:

- Slack
- Discord
- Telegram
- PushBullet
- Dbus
- Mail
- Custom notifications to a given URL in JSON or XML format

## Contributing

Feel free to make a feature request or if you have Python experience; pull requests are welcome
too!

## Troubleshooting

### Could not connect to Docker Engine

Did you mount the Docker socket to the container? Check out the [example](#usage) if you want
to know how.

In the special case that you are accessing Docker over TCP, you should place Disrupt in the
same network as your TCP socket. Using a Docker socket proxy (like
[docker-socket-proxy](https://hub.docker.com/r/tecnativa/docker-socket-proxy/), or
[sockguard](https://github.com/buildkite/sockguard)) is recommended for enhanced security
in this case. You could then configure the proxy to only allow `GET` requests for service info,
for example. That way, if the Disrupt container gets compromised it can't do any harm to the
cluster in the form of modifications/destructive instructions.

## Support

If you're having trouble getting Disrupt to work, we have a
[Discord](https://discord.gg/tDf2yBg) for support and questions.
