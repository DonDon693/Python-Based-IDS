# Python-Based-IDS
Hello! I created an IDS using Python and although it's not exactly good in its creation I helped understand what an IDS is and does a whole lot more. I didn't use any threat feed I simply put a few strings it should look out for and that's considering nothing is encrypted so PLEASE DO NOT IMPLEMENT THIS AS YOUR IDS. Anyway here is a breakdown of my code which I used ChatGpt 3.5 for: 

```python
import re
```
We start by importing the `re` module, which provides support for working with regular expressions. Regular expressions are powerful tools used for pattern matching in strings.

```python
MALICIOUS_PATTERN = r"(malicious|string|pattern|1=1)"
```
Here, we define the `MALICIOUS_PATTERN` variable as a regular expression string. The pattern `"(malicious|string|pattern|1=1)"` will match any occurrence of the words "malicious," "string," or "pattern," and also the specific string "1=1" which is infamously used in SQL injection attacks. You can modify this pattern to match other potential malicious strings.

```python
def detect_malicious_payload(payload):
    """
    Function to detect malicious strings in the payload using regular expressions.
    """
    payload_lower = payload.lower()
    match = re.search(MALICIOUS_PATTERN, payload_lower)

    if match:
        return True
    else:
        return False
```
The `detect_malicious_payload` function takes a `payload` as input and uses regular expressions to detect the presence of malicious strings.

1. `payload.lower()`: The function converts the payload to lowercase using the `lower()` method. This ensures a case-insensitive match since the regular expression pattern is also case-insensitive.

2. `re.search(MALICIOUS_PATTERN, payload_lower)`: The `re.search()` function searches for occurrences of the `MALICIOUS_PATTERN` in the `payload_lower`. If a match is found, it returns a `Match` object, which indicates the presence of a malicious string. If no match is found, it returns `None`.

3. The function then checks if `match` is not `None` (i.e., a match was found) and returns `True`. Otherwise, it returns `False` to indicate that no malicious string was detected.

```python
def monitor_network_traffic(interface="eth0"):
    def receive_packet():
        # Replace this with actual packet capture and extraction logic
        return b"This is a normal payload."

    print(f"Started monitoring network traffic on interface {interface}...")
    try:
        while True:
            packet = receive_packet()
            payload = packet.decode('utf-8', errors='ignore')

            if detect_malicious_payload(payload):
                print("Malicious pattern detected in the network traffic!")
                # Implement your IPS action here, e.g., drop packet, alert, etc.
            else:
                print("Normal network traffic.")
    except KeyboardInterrupt:
        print("Stopped monitoring network traffic.")
```
The `monitor_network_traffic` function is the main function responsible for monitoring network traffic.

1. We have a nested function `receive_packet()` here, which simulates the reception of network packets. In a real-world implementation, you should replace this with actual packet capture and extraction logic from the network interface.

2. We print a message indicating that the monitoring has started for the specified network interface.

3. The function enters an infinite loop (`while True`) to continuously monitor network traffic.

4. Within the loop, we call the `receive_packet` function to get the next network packet.

5. The packet's payload is converted to a string using `packet.decode('utf-8', errors='ignore')`. The `'utf-8'` encoding is commonly used for plain text payloads, but the actual encoding may vary depending on your use case.

6. We then call the `detect_malicious_payload` function to check for the presence of malicious strings in the payload, including the "1=1" query for SQL injection detection.

7. If a malicious pattern is detected (i.e., `detect_malicious_payload` returns `True`), we print a message indicating the detection and may implement an appropriate response or action (e.g., an IPS action like dropping the packet or sending an alert).

8. If no malicious pattern is found, we print a message indicating that the network traffic is normal.

9. The loop continues indefinitely until the user interrupts it (usually by pressing `Ctrl + C`), in which case we catch the `KeyboardInterrupt` exception and print a message indicating that the monitoring has stopped.

Again, please remember that this is a basic example meant for educational purposes, and a production-grade IDS/IPS would require more sophisticated techniques, handling of different protocols, and comprehensive security considerations.
