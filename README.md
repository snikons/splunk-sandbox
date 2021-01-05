# Splunk sandbox on macOS

## Setup

```bash
docker pull splunk/splunk:latest
mkdir -p data/splunk/etc
mkdir -p data/splunk/var
chmod 777 data/splunk/etc
chmod 777 data/splunk/var

# To fix the following error on startup
# homePath='/opt/splunk/var/lib/splunk/audit/db' of index=_audit on
# unusable filesystem.
echo "OPTIMISTIC_ABOUT_FILE_LOCKING=1" > \
data/splunk/etc/splunk-launch.conf

# To fix the following error when uploading data
# Upload failed with ERROR : Read Timeout
mkdir -p data/splunk/etc/system/local
chmod 777 data/splunk/etc/system/local
echo "[diskUsage]" > data/splunk/etc/system/local/server.conf
echo "minFreeSpace = 10" >> data/splunk/etc/system/local/server.conf

docker run -d -p 8000:8000 \
-e "SPLUNK_START_ARGS=--accept-license" \
-e "SPLUNK_PASSWORD=<password>" \
--name splunk \
-v $PWD/data/splunk/etc:/opt/splunk/etc \
-v $PWD/data/splunk/var:/opt/splunk/var \
splunk/splunk:latest
```

## Web access

http://localhost:8000 with `admin:<password>`

## Configure

- To disable usage data sharing, click Settings > Instrumentation
and then click the gear icon next to Usage Data.

