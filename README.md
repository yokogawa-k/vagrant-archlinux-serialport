# Vagrant(vbox) でシリアルポートを使う場合にどのように設定すべきかのメモ

## 環境

- Arch Linux 4.1.2（普段使わないからバージョンが正しいか不明）
- VritualBox 5.0.0
- Vagrant 1.7.4

## 使い方

このレポジトリを clone した環境で `vagrant up` する。

systemd から `ttyS0` をオープンする agetty のインスタンスが上がるように設定する。

```console
$ sudo -i
# cd /etc/systemd/system/getty.target.wants/
# ln -snvf /usr/lib/systemd/system/getty@.service getty@ttyS0.service
# systemctl start getty@ttyS0.service
```

ホストや外部マシンからつなぐ。

```console
host:~$ socat -d -d - tcp:localhost:9999
```

## その他

### tcp 以外を使う方法

[vboxmanage のマニュアル](https://www.virtualbox.org/manual/ch08.html#vboxmanage-modifyvm-other) を参考にパラメータを変更すればよい。設定によっては、named pipe やホストのデバイス経由でつなぐことが出来るはず。

### エコーバックとか

どうしても気になるなら、socat で pty を生成し、tcp なり pipe なりから繋いであげる。そして、pty に screen で接続するとうまくやってくれるらしい。

参考サイト
- [VirtualBox / VMware Player のシリアルコンソールを使う - Qiita](http://qiita.com/albatross/items/8404215ada12f562fe35#2-6)
- [socat を使う (シリアル-TCP変換, etc.) [MA-E/SA Developers' Wiki]](http://ma-tech.centurysys.jp/doku.php?id=mae3xx_tips:use_socat:start)

エコーバックは socat 単体でも回避できそうな気がするが調べてないので不明
