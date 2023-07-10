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

## TCP_NODELAYがセットされていない場合

```
0.000986 0.000253 IP 192.168.1.41.60998 > 192.168.1.40.1234: Flags [P.], seq 1:11, ack 1, win 502, options [nop,nop,TS val 3411187273 ecr 992006031], length 10
0.001036 0.000050 IP 192.168.1.40.1234 > 192.168.1.41.60998: Flags [.], ack 11, win 509, options [nop,nop,TS val 992006032 ecr 3411187273], length 0
0.003443 0.002407 IP 192.168.1.40.1234 > 192.168.1.41.60998: Flags [P.], seq 1:21, ack 11, win 509, options [nop,nop,TS val 992006035 ecr 3411187273], length 20
0.004027 0.000584 IP 192.168.1.41.60998 > 192.168.1.40.1234: Flags [.], ack 21, win 502, options [nop,nop,TS val 3411187276 ecr 992006035], length 0
0.004088 0.000061 IP 192.168.1.40.1234 > 192.168.1.41.60998: Flags [P.], seq 21:41, ack 11, win 509, options [nop,nop,TS val 992006035 ecr 3411187276], length 20
0.004593 0.000505 IP 192.168.1.41.60998 > 192.168.1.40.1234: Flags [.], ack 41, win 502, options [nop,nop,TS val 3411187277 ecr 992006035], length 0
1.005496 1.000903 IP 192.168.1.41.60998 > 192.168.1.40.1234: Flags [P.], seq 11:21, ack 41, win 502, options [nop,nop,TS val 3411188278 ecr 992006035], length 10
1.005943 0.000447 IP 192.168.1.40.1234 > 192.168.1.41.60998: Flags [P.], seq 41:61, ack 21, win 509, options [nop,nop,TS val 992007037 ecr 3411188278], length 20
1.006635 0.000692 IP 192.168.1.41.60998 > 192.168.1.40.1234: Flags [.], ack 61, win 502, options [nop,nop,TS val 3411188279 ecr 992007037], length 0
1.006691 0.000056 IP 192.168.1.40.1234 > 192.168.1.41.60998: Flags [P.], seq 61:81, ack 21, win 509, options [nop,nop,TS val 992007038 ecr 3411188279], length 20
```

length 20のパケットは連続ですぐ送られている訳ではなく、2番目の20バイトは
ackのあとに送られている。

## TCP_NODELAYがセットされている場合

```
0.000885 0.000157 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [P.], seq 1:11, ack 1, win 502, options [nop,nop,TS val 3413508026 ecr 994326785], length 10
0.000930 0.000045 IP 192.168.1.40.1234 > 192.168.1.41.32778: Flags [.], ack 11, win 509, options [nop,nop,TS val 994326786 ecr 3413508026], length 0
0.003582 0.002652 IP 192.168.1.40.1234 > 192.168.1.41.32778: Flags [P.], seq 1:21, ack 11, win 509, options [nop,nop,TS val 994326788 ecr 3413508026], length 20
0.003960 0.000378 IP 192.168.1.40.1234 > 192.168.1.41.32778: Flags [P.], seq 21:41, ack 11, win 509, options [nop,nop,TS val 994326789 ecr 3413508026], length 20
0.004163 0.000203 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [.], ack 21, win 502, options [nop,nop,TS val 3413508029 ecr 994326788], length 0
0.004459 0.000296 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [.], ack 41, win 502, options [nop,nop,TS val 3413508029 ecr 994326789], length 0
1.004999 1.000540 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [P.], seq 11:21, ack 41, win 502, options [nop,nop,TS val 3413509030 ecr 994326789], length 10
1.005436 0.000437 IP 192.168.1.40.1234 > 192.168.1.41.32778: Flags [P.], seq 41:61, ack 21, win 509, options [nop,nop,TS val 994327790 ecr 3413509030], length 20
1.005708 0.000272 IP 192.168.1.40.1234 > 192.168.1.41.32778: Flags [P.], seq 61:81, ack 21, win 509, options [nop,nop,TS val 994327790 ecr 3413509030], length 20
1.006032 0.000324 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [.], ack 61, win 502, options [nop,nop,TS val 3413509031 ecr 994327790], length 0
1.006241 0.000209 IP 192.168.1.41.32778 > 192.168.1.40.1234: Flags [.], ack 81, win 502, options [nop,nop,TS val 3413509031 ecr 994327790], length 0
```

length 20のパケットは連続ですぐ送られている。
