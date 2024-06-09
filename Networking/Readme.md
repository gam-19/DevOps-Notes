### Network Security Firewall

1. **Stateless Firewalls**: 
   * Filter packets without regard to the context of the traffic. 
   * They look at the packets individually and make **decisions based on the source and destination addresses**, **ports**, or **protocols**. 
   * They do not keep track of the state of connections.

2. **Stateful Firewall**: 
   * Keeps track of the state of network connections (such as TCP streams, UDP communication) traveling across it. 
   * Capable of monitoring all sessions (traffic) and determine whether packets are part of an existing connection, a new connection, or not part of any connection. 
   * Only packets matching a known active connection are allowed by the firewall; others are rejected.
   * This makes them effective in protecting against certain types of attacks, such as spoofing attacks.   

3. **Next-Generation Firewalls (NGFWs)**: 
   * More advanced firewalls that do everything a stateful firewall does, but also include additional features like:
     * IPS (intrusion prevention)
     * SSL and SSH inspection
     * Deep-packet inspection
     * Reputation-based malware detection.

4. **Web Application Firewalls (WAFs)**: 
   * Specifically designed to protect web applications from attacks such as SQL injection, cross-site scripting (XSS), and file inclusion.
