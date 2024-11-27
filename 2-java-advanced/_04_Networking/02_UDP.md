### UDP Protocol

What is UDP?

UDP stands for User Datagram Protocol. UDP is part of the Internet protocol suite used by programs running on different computers across a network. Unlike TCP/IP, UDP is used to send small packets called datagrams, allowing for faster transmission. However, UDP does not provide error checking, so it does not guarantee data integrity.

How UDP Works

UDP works similarly to TCP, but it does not provide error checking when transmitting packets.

When an application uses UDP, the packets are simply sent to the recipient. The sender does not wait to ensure that the recipient has received the packet; it continues sending the next packets. If the recipient misses any UDP packets, those packets are lost because the sender will not resend them. This means that devices can communicate faster.

Applications of UDP

UDP is used when speed is prioritized and error correction is not necessary. For example, UDP is commonly used for live streaming and online gaming.

Example: Suppose you are watching a live video stream, which is typically broadcast using UDP rather than TCP. The server sends a continuous UDP stream to the computers that are watching. If you lose connection for a few seconds, the video may pause or lag momentarily and then continue from where it left off. If a few UDP packets are lost, the video or audio may become slightly distorted as the video continues playing without missing data.

This works similarly in online games. If you miss some UDP packets, the player's characters might appear in different locations on the map when you receive the newer UDP packets.