# DNS 
Definition: DNS (Domain Name System) is the digital phonebook of the internet that translates human-readable website names into machine-readable IP addresse
Working: DNS process can be broken down into several steps, ensuring that users can access websites by simply typing a domain name into their browser.
- User Input: The user enters a domain name (e.g., www.geeksforgeeks.org) in the browser.
- Local Cache Check: The browser or OS checks its cache for a stored IP address.
- DNS Resolver Query: If not found, the request is sent to a DNS resolver (usually by ISP).
- Root Server Query: The resolver queries a root server, which points to the correct TLD server.
- TLD Server Response: The TLD server directs the resolver to the domain’s authoritative server.
- Authoritative Server Response: The authoritative server returns the actual IP address.
- Final Response: The resolver sends the IP back to the user, and the browser connects to the server.
