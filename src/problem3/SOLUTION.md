Provide your solution here:

Check
1. Check Memory Usage Details
Command: free -h
check total, used, and free memory, including swap usage
2. Identify Processes Consuming Memory
Command: ps aux --sort=-%mem | head -10
lists the top 10 memory-consuming processes
3. Check System Logs for Errors
Command: journalctl -xe | tail -50
Command: dmesg | grep -i oom
look for OOM (Out of Memory) Killer logs, which indicate that processes are being killed due to memory exhaustion.
4. Check NGINX Worker Processes
COmmand: nginx -T | grep worker_processes
If worker_processes is too high, each worker may be consuming too much memory.
5. Analyze Connection
netstat -an | grep :80 | wc -l
counts the number of active connections to NGINX. If it’s abnormally high, it could be a traffic surge or DDoS attack.
6. Check Nginx Version
Command: nginx -V
If using the old nginx has expired. It is likely to be leak memory.

Fixes
Case 1: Nginx has too many processes (in check.4)
Worker_processes is too high, consuming a lot of memory.
edit Nginx config file: /etc/nginx/nginx.conf
set worker_processes auto;
-> The system automatically adjusts according to the number of CPUs.

Case 2: Hight incoming traffic(could be a valid connection or DDOS attack) (in check.5)
edit Nginx config file: /etc/nginx/nginx.conf
limit_conn_zone $binary_remote_addr zone=addr:10m;

server {
    location /download/ {
        limit_conn addr 10;
    }

-> The above sample code allows the maximum number of connections to 10, or can use fail2ban to block IP send much requests.

Case 3:NGINX Memory Leak or Bug (in check.6)
This error occurs due to the use of Nginx too old or expired
-> sudo apt update && sudo apt

Note: Whatever the error, after finishing, you must restart the nginx and check the logs again
command: systemctl restart nginx
command: journalctl -xe