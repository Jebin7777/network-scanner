import socket
import threading
import argparse
from datetime import datetime

# Common ports and their service names
COMMON_SERVICES = {
    21: "FTP",
    22: "SSH",
    23: "Telnet",
    25: "SMTP",
    53: "DNS",
    80: "HTTP",
    110: "POP3",
    135: "RPC",
    139: "NetBIOS",
    143: "IMAP",
    443: "HTTPS",
    445: "SMB",
    3306: "MySQL",
    3389: "RDP",
    5900: "VNC",
    8080: "HTTP-Proxy",
    8443: "HTTPS-Alt",
}

open_ports = []
lock = threading.Lock()

def get_service(port):
    if port in COMMON_SERVICES:
        return COMMON_SERVICES[port]
    try:
        return socket.getservbyport(port)
    except:
        return "Unknown"

def scan_port(ip, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(0.5)
        result = sock.connect_ex((ip, port))
        sock.close()
        if result == 0:
            service = get_service(port)
            with lock:
                open_ports.append(port)
                print(f"  [OPEN]  Port {port:<6} --> {service}")
    except:
        pass

def main():
    parser = argparse.ArgumentParser(description="Network Port Scanner")
    parser.add_argument("ip", help="Target IP address")
    parser.add_argument("--start", type=int, default=1, help="Start port (default: 1)")
    parser.add_argument("--end", type=int, default=1024, help="End port (default: 1024)")
    args = parser.parse_args()

    ip = args.ip
    start_port = args.start
    end_port = args.end

    print("-" * 50)
    print(f"  Target   : {ip}")
    print(f"  Ports    : {start_port} - {end_port}")
    print(f"  Started  : {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
    print("-" * 50)

    threads = []
    for port in range(start_port, end_port + 1):
        t = threading.Thread(target=scan_port, args=(ip, port))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    print("-" * 50)
    print(f"  Scan complete. {len(open_ports)} open port(s) found.")
    print("-" * 50)

main()
