# Network Port Scanner

A Python-based network port scanner that identifies open ports and running services on a target host.

## Features
- Scans ports 1-1024 by default
- Custom port range using --start and --end flags
- Identifies service names (SSH, HTTP, FTP etc.)
- Fast scanning using multi-threading
- Clean timestamped output

## Requirements
- Python 3.x
- No external libraries needed

## Usage

Basic scan:
python3 scanner.py 127.0.0.1

Custom port range:
python3 scanner.py 192.168.1.1 --start 1 --end 500

## Example Output

--------------------------------------------------
  Target   : 127.0.0.1
  Ports    : 1 - 1024
  Started  : 2026-06-04 02:52:38
--------------------------------------------------
  [OPEN]  Port 22     --> SSH
  [OPEN]  Port 80     --> HTTP
--------------------------------------------------
  Scan complete. 2 open port(s) found.
--------------------------------------------------

## Skills Demonstrated
- Python programming
- TCP/IP networking concepts
- Socket programming
- Multi-threading
- Command line argument parsing
