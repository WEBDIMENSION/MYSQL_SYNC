# MySQL SYNC

## Overview

Remote MySQL のデータを ローカル( Docker Container) へインポート。

## Processing
- SSH で remote login
- Remote で mysqldump 実行
- SCP でローカルへコピー
- SED で  dumpfile 書き換え *任意
- ローカルのMySQLへ Restore
- ローカル用のSQL実行(After SQL) *任意

## Usage

```bash
docker-compose mysql_sync sync
# more
```
[mysql5.1 ReadMe](./mysql51/Docker/mysql_sync/README.md)

[mysql5.7 ReadMe](./mysql57/Docker/mysql_sync/README.md)

[mysql8 ReadMe](./mysql8/Docker/mysql_sync/README.md)
