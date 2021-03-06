# memory-monitor

Monitors process group and system memory usage and sends warning emails.

## Requirements

* Linux system with `Pss` fields in `/proc/PID/smaps`
* `mail`
* `ps`
* `free`
* Python packages in `requirements.txt`

## Configuration

Default configuration is stored in `config.default`. `config.yml` will be created on first run.

Sample configuration:

```
Configuration (config.yml):
  System memory: 503.8GB
  System critical warning memory threshold: 50.4GB (10.00%)
  System critical process termination: Inactive
  Process group warnings:
    50.0% of memory (251.9GB), warn after 0 hours
    20.0% of memory (100.8GB), warn after 6 hours
    10.0% of memory (50.4GB), warn after 24 hours
    5.0% of memory (25.2GB), warn after 168 hours
    1.0% of memory (5.0GB), warn after 672 hours
  Processes considered idle after: 360 seconds
  Processes considered idle with CPU usage less than: 5.0%
  Processes polled every: 600 seconds
  Maximum warning frequency: 3600 seconds
  Warnings will be sent to: your@email.com
```

## `systemd`

You can run `mem-monitor` automatically on boot with `systemd`. A sample service file is included. You can set it up as follows:

```
git clone https://github.com/scottgigante/memory-monitor
cd memory-monitor
cp config.default config.yml
# modify the config as necessary
vim config.yml

sudo cp mem-monitor.service /etc/systemd/system/
sudo mkdir /var/log/mem_monitor
sudo mkdir /etc/systemd/system/mem_monitor/
sudo cp mem_monitor.py /etc/systemd/system/mem_monitor/
sudo cp config.yml /etc/systemd/system/mem_monitor/
sudo systemctl daemon-reload
sudo systemctl enable mem-monitor
sudo systemctl start mem-monitor
```
