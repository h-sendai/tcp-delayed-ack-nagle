# TCP Delayed Ack and Nagle algorithm

```
クライアント            サーバー

 10 バイト ----------->
           <----------- 20 バイト
           <----------- 20 バイト

(スリープ1秒)

 10 バイト ----------->
           <----------- 20 バイト
           <----------- 20 バイト
```