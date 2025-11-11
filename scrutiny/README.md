# Scrutiny - (S.M.A.R.T. Visualizer)
Docker Compose for running **[Scrutiny](https://github.com/AnalogJ/scrutiny)**

![it works](it-works.png)


## Overview
Scrutiny collects and displays health information from all drives connected to the host.  
I am using the **omnibus image** (includes InfluxDB) so it is just one container.

## Reason
**[smartmontools](www.smartmontools.org)** is spectacular software.
However, taking the time to wade through 
```bash
sudo smartctl -a /dev/<whatever-drive>
```
and actually coming up with a good idea of how all of your drives are doing, and more importantly how worried you should be, requires a lot of time.

I used to use **[Hard Drive Sentinel](https://www.hdsentinel.com/hard_disk_sentinel_linux.php)** for this purpose, however it has not been an active project in years.

Scrutiny works really well and I think actually uses Backblaze statistics to give you a breakdown of what the statistical likelihood of one of your drives failing is based on the value of a given attribute, which is interesting.

The problem is `smartctl` requires root privileges, and therefore creating and using a container without `privileged: true` is somewhat challenging.

"But Austin you did `cap_add: - SYS_ADMIN` and `- SYS_RAWIO`", which is true, and while that is less than ideal from a security perspective, it is **much better** than `privileged: true`. Additionally, it is **also much better** than having a drive that is screaming at you that it is going to die and not knowing because you do not have time to wade into the minutiae of the S.M.A.R.T. data (though if you think S.M.A.R.T. will save you from drive failure/data loss, it will not).

When you are doing a thing, and it requires effort, a repository is a good idea:
- It will likely save you time in the future
- And someone else might benefit as well

## Usage
Clone the repository and run:
```bash
docker compose up -d
```

Then open http://localhost:8088 (or whatever host port you mapped)

## Configuration
- Container runs as root to access /dev/nvme* and /dev/sd*.
- Added minimal capabilities (SYS_ADMIN, SYS_RAWIO) so it can read data without being fully privileged.
- Data is stored locally in:
  - ./config → Scrutiny config
  - ./influxdb → Time-series data

## Notes
Change the host port in the compose file if 8088 is in use.

Drives must be visible on the host (smartctl should work locally).

Tested on Debian 13 with Docker Compose v2.

This is for personal use / homelab monitoring — no external network exposure.

---

&nbsp;

**466f724a616e6574**
