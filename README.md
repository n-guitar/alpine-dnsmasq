# alpine-dnsmasq

-   Docker で DNS を起動し、mac からワイルドカード DNS を利用する例。

## Docker image build

```bash
$ docker build -t dnsmasq:sample .
```

## DNS config

-   以下の例は`*test-domain`のワイルドカードを`ipaddress`(例:192.168.xx.xx)宛

```bash
$ vi dnsmasq.conf
port=53
address=/.test-domain/ipaddress
```

## Docker run

```bash
$ docker run -d -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN \
-v $PWD/dnsmasq.conf:/etc/dnsmasq.conf \
--name dnsmasq dnsmasq:sample
```

## lookup test DNS 指定

```bash
$ nslookup test.test-domain 127.0.0.1
Server: 127.0.0.1
Address: 127.0.0.1#53

$ dig test.test-domain @127.0.0.1
```

## mac で DNS を記載する場合の例

-   「ネットワークの環境設定を開く」-> 「Wi-Fi」 -> 「詳細」 -> 「DNS」タブから 127.0.0.1 を追加<br>
-   DNS キャッシュクリア

```bash
$ sudo killall -HUP mDNSResponder
```

## lookup test

```bash
$ nslookup test.test-domain
Server: 127.0.0.1
Address: 127.0.0.1#53

$ dig test.test-domain
```
