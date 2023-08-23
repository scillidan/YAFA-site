---
draft: true
---

## ArchiveBox
### Note

```
archivebox manage createsuperuser
archivebox server 127.0.0.1:8010
```

### Reference

1. https://docs.archivebox.io/en/master/Publishing-Your-Archive.html

## Jellyfin
### Tools

- [mnamer](https://github.com/jkwill87/mnamer)
- [sub-batch](https://github.com/kl/sub-batch)

### Notes

```cmd
mnamer -b .
sub-batch rename --subarea ".+"
```

## Komga
### Tools

- [Directory Opus](https://www.gpsoft.com.au/)
- [Xnconvert](https://www.xnview.com/en/xnconvert/)
- [FastStone](https://www.faststone.org/FSViewerDetail.htm)
- [Advanced Renamer](https://www.advancedrenamer.com)
- [To CBZ](https://github.com/italomaia/to-cbz)
- [Komga Cover Extractor](https://github.com/zachstultz/komga-cover-extractor)

### CMD

```
for /d %%X in (*) do arenc.exe -e ".../v01.aren" -t folders -p . -m rename ^
	&& for /d %%X in (*) do arenc.exe -e ".../001.aren" -t files -p %%X -m rename ^
	&& for /d %%X in (*) do cp %%X/001.jpg %%X/cover.jpg ^
	&& for /d %%X in (*) do py39 to-cbz/to_cbz.py %%X ^
	&& py39 D:/binp/komga-cover-extractor/komga_cover_extractor.py -c "True" -cq "70" -p .
```

## Miniflux
### Tools

- [github-search-rss](https://github.com/azu/github-search-rss)
- [miniflux](rss](miniflux](https://github.com/miniflux/v2)
- [postgresql](https://www.postgresql.org)
- [postgrest](https://postgrest.org/en/stable)
- [reminiflux](https://github.com/reminiflux/reminiflux)
- [mprocs](https://github.com/pvolok/mprocs)
- [linkding](https://github.com/sissbruecker/linkding)
- [linkding-injector](https://github.com/Fivefold/linkding-injector)
- [go-windows-shortcut](https://github.com/nyaosorg/go-windows-shortcut)


### CMD

```cmd
initdb --username=--USERNAME DIRPATH
postgres -D DIRPATH
# pg_ctl register -D DIRPATH -N miniflux -U miniflux -P secret
miniflux -config-file .../miniflux.conf
```

the demo `miniflux.conf`:

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

backup data:

```
pg_dump -U miniflux -h localhost -p 5432 -F t miniflux > D:\ini\miniflux\miniflux.tar
```

### Reference

- [Configuration Parameters](https://miniflux.app/docs/configuration.html)