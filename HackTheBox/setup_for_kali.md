# Kaliのセットアップ

## Meow

[openvpn_参考サイト](https://www.ovpn.com/en/guides/ubuntu-cli)

## 2. Download components

```
apt-get install openvpn unzip
```

## 3. Download the configuration you want

```bash
cd /tmp && wget https://files.ovpn.com/ubuntu_cli/ovpn-jp-tokyo.zip && unzip ovpn-jp-tokyo.zip && mkdir -p /etc/openvpn && mv config/* /etc/openvpn && chmod +x /etc/openvpn/update-resolv-conf && rm -rf config && rm -f ovpn-jp-tokyo.zip
```

## 4. Enter your login credentials

```bash
echo "CHANGE TO YOUR USERNAME" >> /etc/openvpn/credentials
echo "CHANGE TO YOUR PASSWORD" >> /etc/openvpn/credentials
```

## 5. Start OpenVPN and see that everything works

```bash
openvpn --config /etc/openvpn/ovpn.conf --daemon
```

## 6. Verify that the connection was successful

```bash
curl https://www.ovpn.com/v2/api/client/ptr
```

* curlでエラーになったけど、以下コマンドでvpn接続できたのでOK

```bash
sudo openvpn ~/Desktop/starting_point_rassio.ovpn
ip a # tun0
ping <ハック対象のサーバーのIPアドレス>
```

