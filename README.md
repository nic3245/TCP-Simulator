# TCP-Simulator
A reliable transport protocol simulation
# Description
A sender and receiver program that implements a reliable transport protocol over some network, real or simulated. <br>
The receiver program is run first and indefinitely, and it prints out the port it is on to STDERR. It prints any data sent to it to STDOUT. <br>
The sender program is run next and is given arguments of the IP address and the port to send to, the input to send via STDIN, then dies after the message is received.
# Example Usage
$ ./3700recv <br>
$ ./3700send <recv_host> <recv_port <br>
$ Hello! How are you today? <br>
$ "Hello! How are you today?" <br>
