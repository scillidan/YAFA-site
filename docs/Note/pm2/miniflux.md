---
tags: [reference#, tool#, cmd#]
---

## Tool

- [github-search-rss](https://github.com/azu/github-search-rss)
- [miniflux](rss](miniflux](https://github.com/miniflux/v2)
- [postgresql](https://www.postgresql.org)
- [postgrest](https://postgrest.org/en/stable)
- [reminiflux](https://github.com/reminiflux/reminiflux)
- [mprocs](https://github.com/pvolok/mprocs)
- [linkding](https://github.com/sissbruecker/linkding)
- [linkding-injector](https://github.com/Fivefold/linkding-injector)
- [go-windows-shortcut](https://github.com/nyaosorg/go-windows-shortcut)

### Tool for Feeds

- [watch-rss](https://github.com/azu/watch-rss)
- [ProductHunt daily RSS feed](https://github.com/headllines/producthunt-daily-rss)
- [F-Droid_Newapps_RSS](https://github.com/yzqzss/F-Droid_Newapps_RSS)

## CMD

```sh
initdb --username=--USERNAME DIRPATH
postgres -D DIRPATH
# pg_ctl register -D DIRPATH -N miniflux -U miniflux -P secret
miniflux -config-file .../miniflux.conf
```

The demo `miniflux.conf`:

```
DATABASE_URL=user=miniflux password=secret dbname=miniflux sslmode=disable
RUN_MIGRATIONS=1
POLLING_FREQUENCY=60
POSTGRES_USER=miniflux
POSTGRES_PASSWORD=secret
CREATE_ADMIN=1
ADMIN_USERNAME=USERNAME
ADMIN_PASSWORD=PASSWORDDEBUG=on
WORKER_POOL_SIZE=10
```

Backup data:

```sh
pg_dump -U miniflux -h localhost -p 5432 -F t miniflux > D:\ini\miniflux\miniflux.tar
```

### Reference

- [Configuration Parameters](https://miniflux.app/docs/configuration.html)
- [Filtering Rules](https://miniflux.app/docs/rules.html#filtering-rules)
- [Keyboard shortcuts](https://github.com/yang991178/fluent-reader/wiki/Support#keyboard-shortcuts)