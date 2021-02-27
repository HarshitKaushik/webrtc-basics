# Concepts for multi-device live streaming

## Selective Forwarding Middlebox

Another method for handling media in the RTP mixer is to "project", or make available, all potential RTP sources (SSRCs) into a per-endpoint, independent RTP session.  The middlebox can select which of the potential sources that are currently actively transmitting media will be sent to each of the endpoints. This is similar to the Media-Switching Mixer but has some important differences in RTP details.

## Secure Real-time Transport Protocol (SRTP)

The Secure Real-time Transport Protocol (SRTP) is a profile for Real-time Transport Protocol (RTP) intended to provide encryption, message authentication and integrity, and replay attack protection to the RTP data in both unicast and multicast applications. It was developed by a small team of Internet Protocol and cryptographic experts from Cisco and Ericsson. It was first published by the IETF in March 2004 as RFC 3711.

## Mediasoup v3

- Horizontal scalability - By using the new pipe transports in v3, two mediasoup routers running in the same or different hosts can be interconnected at media level, increasing the broadcasting capabilities by enabling usage of multiple CPU cores even in different machines. More info about this here.
- Multiple binding IPs - In mediasoup v2 just a static IPv4 and IPv6 pair can be assigned to all transports. In v3, instead, each transport can be provided with multiple and different IPv4 and/or IPv6 addresses. This enables scenarios in which media is conveyed through public and private network interfaces.
- Sender side BWE - mediasoup v3 implements sender side bandwidth estimation to automatically switch between spatial/temporal layers in consumers thus accommodating the total transport bitrate to the bandwidth available in the receiver endpoint.
- Stream score - mediasoup v3 notifies the application with scores for every RTP stream (in producers and consumers), allowing the application to know how the overall transmission quality is in every sender and receiver.
- New DirectTransport - It represents a direct connection between the mediasoup Node.js process and a Router instance in a mediasoup-worker subprocess. It enables direct transmission of data messages between the Node.js application and the mediasoup Router by using DataProducers and DataConsumers. It also enables direct injection on RTP and RTCP packet from Node.js by using the producer.send(rtpPacket) and consumer.on('rtp') API (plus directTransport.sendRtcp(rtcpPacket) and directTransport.on('rtcp')).
- Notice that there are no “peers” per se in mediasoup. However the application may wish to define “peers”, which may identify and associate a specific user account, WebSocket connection, metadata, and a set of mediasoup transports, producers, consumers. data producers and data consumers.
- When a transport, producer, consumer, data producer or data consumer is closed in client or server side (e.g. by calling close() on it), the application should signal its closure to the other side which should also call close() on the corresponding entity.
- Unlike other existing SFU implementations, mediasoup is not a standalone server but an unopinionated Node.js module which can be integrated into a larger application.