---
draft: true
title: Quic Research
categories:
  - Webtech
---
## QUIC support at LBs

https://cloud.google.com/load-balancing/docs/https/#QUIC


## Quic library support

https://crates.io/crates/quinn
https://github.com/litespeedtech/lsquic-client
https://github.com/lucas-clemente/quic-go

Try to write a small quic client

—

## Setup a quic server

0. try to get chrome to serve quic pages from a non google quic server (done)

1. start quic server:

./out/Default/quic_server  --quic_response_cache_dir=/tmp/quic-data/www.example.org --certificate_file=net/tools/quic/certs/out/leaf_cert.pem --key_file=net/tools/quic/certs/out/leaf_cert.pkcs8

caddy -host chatyuk.com -quic


1. enable quic chrome://flags/
2. chrome://net-export/
3. https://netlog-viewer.appspot.com/#import


/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --user-data-dir=/tmp/chrome-profile \
  --no-proxy-server \
  --enable-quic \
  --origin-to-force-quic-on=www.mohangk.com:443 \
  https://www.mohangk.com

  --host-resolver-rules='MAP www.example.org:443 127.0.0.1:6121' \
  
  
  https://groups.google.com/a/chromium.org/forum/#!topic/proto-quic/8077taFX8kA


-------

https://cdn-g-production-vidio.akamaized.net/1x1.png
cdn-g-production-vidio.akamaized.net
——


1. write a simple quic client
2. write a quic client library
3. try out tdlib
4. improve send 2 kindle, see if there is a better way to strip page numbers, add space between titles, remove diagrams?
5. understand how alt-svc works
6. setup mohangk.org using caddyserver and quic on an instance in GCP

chrome://flags/#enable-quic


Example of TCP a HTTP GET Request URL with the alt-svc: quic=”443” HTTP response header

./quic_client  --disable-certificate-verification  https://chatyuk.com

 quicver chatyuk.com
"chatyuk.com:443" supported versions:
     Q044
     Q043
     Q039

https://hnrk.io/quic/
https://github.com/povilasb/quic-version-detector
https://github.com/quicwg/base-drafts/wiki/Implementations


https://h2o.examp1e.net/
https://github.com/h2o/quicly

https://tosbourn.com/getting-os-x-to-trust-self-signed-ssl-certificates/

reading:
https://devsisters.github.io/goquic/

quic performance for streaming
https://2018.packet.video/wp-content/uploads/sites/6/2017/12/3002.pdf
https://www.researchgate.net/publication/325920273_Quickly_Starting_Media_Streams_Using_QUIC

http://downloads.bbc.co.uk/rd/pubs/whp/whp-pdf-files/WHP336.pdf
https://david.choffnes.com/pubs/long-look-at-quic-imc17.pdf
https://www.codavel.com/2018/09/17/quic-vs-tcptls-and-why-quic-is-not-the-next-big-thing/
https://code.fb.com/android/building-zero-protocol-for-fast-secure-mobile-connections/
https://blog.cloudflare.com/the-road-to-quic/
https://medium.com/@nirosh/understanding-quic-wire-protocol-d0ff97644de7
https://ma.ttias.be/googles-quic-protocol-moving-web-tcp-udp/
https://groups.google.com/a/chromium.org/forum/#!topic/proto-quic/G2K2xb2qIOQ


————
## QUIC protocol testing tool 
https://github.com/QUIC-Tracker/quic-tracker


## Quic usage in the wild
https://quic.netray.io/stats.html



## How do client & server detect and negotiate using HTTP2 
Using ALPN that is Application-Layer Protocol Negotiation


https://www.nginx.com/blog/supporting-http2-google-chrome-users/


## IETF vs Google QUIC

- Google-QUIC only ever transported HTTP - in practice it transported what was effectively HTTP/2 frames, using the HTTP/2 frame syntax.
- USE TLS 1.3 instead of custom encryption
- e IETF QUIC protocol architecture was split in two separate layers: the transport QUIC and the "HTTP over QUIC" layer (the latter sometimes referred to as "hq").

