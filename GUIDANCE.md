# Ghost on Wodby

What Wodby sets up for Ghost on this service. Ghost runs from the official `ghost` image; all of the configuration below is passed as environment variables, which Ghost reads as configuration keys with `__` between nested levels.

## Linked services

| Link | Variables |
| --- | --- |
| MySQL database (required) | `database__client`, `database__connection__host`, `database__connection__port`, `database__connection__user`, `database__connection__password`, `database__connection__database` |
| Mail transfer agent (required) | `mail__transport`, `mail__options__host`, `mail__options__port`, `mail__options__secure` |

Mail goes to the linked mail service over SMTP without TLS or credentials. Do not add database or SMTP settings elsewhere.

## URL and server

- `url` is the environment's primary URL. Ghost uses it for links, so it changes with the primary host.
- Ghost listens on `server__host` and `server__port`, port 2368, which is the service's endpoint. `NODE_ENV` is `production`.
- The setting "Email sender" (required) sets `mail__from`.

## Changing configuration

Add or change environment variables on the service, using the same `section__key` form, and deploy the service. Do not edit `config.production.json` in the container: it is not on a volume.

## Data

- The `content` volume is mounted at `/var/lib/ghost/content`: images, themes and other content files. Posts, users and settings are in the linked MySQL database.
- The manifest declares a content import into that directory (`tar`, `tar.gz`, `tgz`, `zip`) and a content backup of it. Neither includes the database.
- The manifest creates no administrator account: the owner account is created in the browser at `/ghost` on first use.
