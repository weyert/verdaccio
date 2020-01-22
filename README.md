# minio-dokku

[min.io](https://min.io/) on Dokku.

See here: https://github.com/slypix/minio-dokku

## Add env vars for access

```
dokku config:set restauranto-s3 MINIO_ACCESS_KEY=$(echo `openssl rand -base64 45` | tr -d \=+ | cut -c 1-20)
dokku config:set restauranto-s3 MINIO_SECRET_KEY=$(echo `openssl rand -base64 45` | tr -d \=+ | cut -c 1-32)
```

## Fix ports and SSL

Order is important because dokku-letsencrypt adds 443 port.

```
dokku proxy:report restauranto-s3
dokku proxy:ports-add restauranto-s3 http:80:9000
dokku proxy:ports-remove restauranto-s3 http:9000:9000
dokku letsencrypt 4estauranto-s3
dokku letsencrypt:cron-job --add restauranto-s3
```

## Persistent storage

```
mkdir -p /var/lib/dokku/data/storage/restauranto-storage
chown -R 32769:32769 /var/lib/dokku/data/storage/restauranto-storage
dokku storage:mount restauranto-s3 /var/lib/dokku/data/storage/restauranto-storage:/data
```

## Configure maximum upload size

```
sudo dokku plugin:install https://github.com/Zeilenwerk/dokku-nginx-max-upload-size.git
dokku config:set restauranto-s3 MAX_UPLOAD_SIZE=100M
```

## License

MIT
