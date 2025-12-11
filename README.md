ceph-scripts
============
This is a fork from [https://github.com/cernceph/ceph-scripts](https://github.com/cernceph/ceph-scripts). Please send your stars to him not me.

Small helper scripts for monitoring/managing a Ceph cluster 

Docker usage for `ceph-gentle-reweight`
---------------------------------------

Build the image from the repo root:

```
docker build -t ceph-gentle-reweight .
```

Run the tool from the image (mount your Ceph config/keys so `ceph` CLI works in the container):

```
docker run --rm \
  -v /etc/ceph:/etc/ceph:ro \
  ceph-gentle-reweight \
  -o osd.1,osd.2 \
  -d 0.01 \
  -t 2 \
  -r
```

Adjust flags as needed (e.g., `-l` max latency, `-b` max backfills, `-p` latency test pool).

Flags (one-line each):
- `-o/--osds`: comma-separated OSD ids to reweight.
- `-l/--latency`: max allowed latency in ms before pausing.
- `-b/--backfills`: max PGs backfilling before pausing.
- `-d/--delta`: weight increment/decrement per step.
- `-t/--target`: target crush weight to reach.
- `-p/--pool`: pool used to measure latency (`rados bench`).
- `-i/--interval`: seconds to sleep between iterations.
- `-s/--start-time`: start of allowed window (`HH:MM`).
- `-e/--end-time`: end of allowed window (`HH:MM`).
- `-a/--allowed-days`: allowed weekdays as ints (`0`=Mon).
- `-r/--really`: actually apply weights (omit for dry-run).

Dry-run example (no reweights executed):

```
docker run --rm \
  -v /etc/ceph:/etc/ceph:ro \
  ceph-gentle-reweight \
  -o osd.1,osd.2 \
  -d 0.01 \
  -t 2
```
