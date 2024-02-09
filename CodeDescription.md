High-level approach: 

On the Sender side, the client waits for responses (ACKs or errors), checks for timeouts of currently sent packets, and waits for our buffer of messages to be smaller than the window, in which case it will read in data from stdin and send it off. If the Sender receives an error response (or no ack in case of timeout), it resends the packets currently in the buffer.

On the Receiver side, the client waits to receive packets, checks their sequence number (drops duplicated packets), and confirms their checksum (sends an error response if the packet has been corrupted). It will save the packet, then it checks whether it can print out the next packet to stdout (do any stored packets have the correct sequence number). The receiver finally sends an ack response if there are no errors with the reception.

Challenges we faced:
One challenge we faced was ACK messages getting corrupted. At first, we didn't realize that was what was happening - it took many log messages before we understood that it was a possibility. After, we struggled with how to fix it - we first tried to send each ACK message twice, but sometimes they both would be corrupted. We then realized we could implement a checksum on this message as well, and if it was wrong, then the sender needs to resend.
Another challenge was getting the performance fast enough. It was tricky to figure out how fast to increase/decrease window size such that it was efficient enough.

Good design:

We implemented a checksum in our protocol, which encrypts the json with sha256 and takes mod 256 of that to reduce it to a single byte.
This is relatively fast although it has a chance of not catching errors due to it being restricted to 8 bits.
We also thought our dictionary usage for keeping track of information allowed for fast retrieval.
Furthermore, by saving the sending time of each message, RTT can be estimated well by measuring the time that the ACK is receiverd for it.
Window size increasing/decreasing by 1 is probably not the most efficient, but it is safe - it is less likely to drop multiple messages.


Testing Overview:

Our code was mainly tested using the given testing files from the command line, having log messages in our code, and also on gradescope for performance.