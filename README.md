# django-myblogapp

## AWS
- ubuntu16を作成
  - `sudo apt-get update`
  - `sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx`
  - `sudo -u postgres psql`
- DATABASE設定
  - `CREATE DATABASE myblogapp;`  //DATABASEを作成
  - `CREATE USER mybloguser WITH PASSWORD 'hogehoge';`  //Djangoにアクセスするためのユーザを作成
  - `ALTER ROLE mybloguser SET client_encoding TO 'utf8';`  //日本語使えるようにエンコードを置き換え
  - `ALTER ROLE mybloguser SET default_transaction_isolation TO 'read committed';`    //実行された結構だけを見にいく設定
  - `ALTER ROLE mybloguser SET timezone TO 'UTC+9';`  //タイムゾーンの設定
  - `GRANT ALL PRIVILEGES ON DATABASE myblogapp TO mybloguser;`   //myblogappデータベースのアクセスをmybloguserに許可
  - `\q`  //抜ける

- 起動プロセス

```
ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.5  37668  5664 ?        Ss   14:08   0:03 /sbin/init
root         2  0.0  0.0      0     0 ?        S    14:08   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    14:08   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   14:08   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    14:08   0:00 [kworker/u30:0]
root         7  0.0  0.0      0     0 ?        S    14:08   0:00 [rcu_sched]
root         8  0.0  0.0      0     0 ?        S    14:08   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    14:08   0:00 [migration/0]
root        10  0.0  0.0      0     0 ?        S    14:08   0:00 [watchdog/0]
root        11  0.0  0.0      0     0 ?        S    14:08   0:00 [kdevtmpfs]
root        12  0.0  0.0      0     0 ?        S<   14:08   0:00 [netns]
root        13  0.0  0.0      0     0 ?        S<   14:08   0:00 [perf]
root        14  0.0  0.0      0     0 ?        S    14:08   0:00 [xenwatch]
root        15  0.0  0.0      0     0 ?        S    14:08   0:00 [xenbus]
root        17  0.0  0.0      0     0 ?        S    14:08   0:00 [khungtaskd]
root        18  0.0  0.0      0     0 ?        S<   14:08   0:00 [writeback]
root        19  0.0  0.0      0     0 ?        SN   14:08   0:00 [ksmd]
root        20  0.0  0.0      0     0 ?        SN   14:08   0:00 [khugepaged]
root        21  0.0  0.0      0     0 ?        S<   14:08   0:00 [crypto]
root        22  0.0  0.0      0     0 ?        S<   14:08   0:00 [kintegrityd]
root        23  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        24  0.0  0.0      0     0 ?        S<   14:08   0:00 [kblockd]
root        25  0.0  0.0      0     0 ?        S<   14:08   0:00 [ata_sff]
root        26  0.0  0.0      0     0 ?        S<   14:08   0:00 [md]
root        27  0.0  0.0      0     0 ?        S<   14:08   0:00 [devfreq_wq]
root        30  0.0  0.0      0     0 ?        S    14:08   0:00 [kswapd0]
root        31  0.0  0.0      0     0 ?        S<   14:08   0:00 [vmstat]
root        32  0.0  0.0      0     0 ?        S    14:08   0:00 [fsnotify_mark]
root        33  0.0  0.0      0     0 ?        S    14:08   0:00 [ecryptfs-kthre
root        49  0.0  0.0      0     0 ?        S<   14:08   0:00 [kthrotld]
root        50  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        51  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        52  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        53  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        54  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        55  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        56  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        57  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        58  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        59  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        60  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        61  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        62  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        63  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        64  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        65  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        66  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        67  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        68  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        69  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        70  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        71  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        72  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        73  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        74  0.0  0.0      0     0 ?        S    14:08   0:00 [scsi_eh_0]
root        75  0.0  0.0      0     0 ?        S<   14:08   0:00 [scsi_tmf_0]
root        76  0.0  0.0      0     0 ?        S    14:08   0:00 [scsi_eh_1]
root        77  0.0  0.0      0     0 ?        S<   14:08   0:00 [scsi_tmf_1]
root        82  0.0  0.0      0     0 ?        S<   14:08   0:00 [ipv6_addrconf]
root        95  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root        96  0.0  0.0      0     0 ?        S<   14:08   0:00 [deferwq]
root       258  0.0  0.0      0     0 ?        S<   14:08   0:00 [raid5wq]
root       287  0.0  0.0      0     0 ?        S<   14:08   0:00 [bioset]
root       310  0.0  0.0      0     0 ?        S    14:08   0:00 [jbd2/xvda1-8]
root       311  0.0  0.0      0     0 ?        S<   14:08   0:00 [ext4-rsv-conve
root       397  0.0  0.0      0     0 ?        S<   14:08   0:00 [iscsi_eh]
root       400  0.0  0.0      0     0 ?        S<   14:08   0:00 [ib_addr]
root       401  0.0  0.0      0     0 ?        S<   14:08   0:00 [ib_mcast]
root       402  0.0  0.0      0     0 ?        S<   14:08   0:00 [ib_nl_sa_wq]
root       403  0.0  0.0      0     0 ?        S<   14:08   0:00 [ib_cm]
root       406  0.0  0.0      0     0 ?        S<   14:08   0:00 [iw_cm_wq]
root       407  0.0  0.0      0     0 ?        S<   14:08   0:00 [rdma_cm]
root       409  0.0  0.2  28352  2688 ?        Ss   14:08   0:00 /lib/systemd/sy
root       413  0.0  0.0      0     0 ?        S    14:08   0:00 [kauditd]
root       440  0.0  0.1  94772  1532 ?        Ss   14:08   0:00 /sbin/lvmetad -
root       471  0.0  0.3  42372  3748 ?        Ss   14:08   0:00 /lib/systemd/sy
systemd+   522  0.0  0.2 100324  2316 ?        Ssl  14:08   0:00 /lib/systemd/sy
root       546  0.0  0.0      0     0 ?        S<   14:08   0:00 [kworker/0:1H]
root       917  0.0  0.2  16124  2116 ?        Ss   14:08   0:00 /sbin/dhclient
daemon    1066  0.0  0.1  26044  1960 ?        Ss   14:08   0:00 /usr/sbin/atd -
root      1067  0.0  0.0   5220   148 ?        Ss   14:08   0:00 /sbin/iscsid
root      1068  0.0  0.3   5720  3504 ?        S<Ls 14:08   0:00 /sbin/iscsid
root      1074  0.0  0.2  28544  2932 ?        Ss   14:08   0:00 /lib/systemd/sy
syslog    1077  0.0  0.3 260628  3220 ?        Ssl  14:08   0:00 /usr/sbin/rsysl
message+  1082  0.0  0.3  42896  3296 ?        Ss   14:08   0:00 /usr/bin/dbus-d
root      1101  0.0  0.5 272944  5672 ?        Ssl  14:08   0:00 /usr/lib/accoun
root      1119  0.0  0.1  95368  1460 ?        Ssl  14:08   0:00 /usr/bin/lxcfs
root      1133  0.0  0.2  26068  2404 ?        Ss   14:08   0:00 /usr/sbin/cron
root      1135  0.0  0.1   4396  1288 ?        Ss   14:08   0:00 /usr/sbin/acpid
root      1163  0.0  0.0  13372   160 ?        Ss   14:08   0:00 /sbin/mdadm --m
root      1166  0.0  0.5 277180  5704 ?        Ssl  14:08   0:00 /usr/lib/policy
root      1251  0.0  0.1  14656  1604 tty1     Ss+  14:08   0:00 /sbin/agetty --
root      1254  0.0  0.1  12840  1612 ttyS0    Ss+  14:08   0:00 /sbin/agetty --
root      1281  0.0  0.3  65512  3916 ?        Ss   14:08   0:00 /usr/sbin/sshd
root      1409  0.0  0.0      0     0 ?        S<   14:08   0:00 [loop0]
root      1519  0.0  2.2 304616 22544 ?        Ssl  14:08   0:00 /usr/lib/snapd/
root      1635  0.0  0.0      0     0 ?        S<   14:08   0:00 [loop1]
root      1702  0.0  1.2 206676 13068 ?        Ssl  14:08   0:00 /snap/amazon-ss
root      1792  0.0  0.4  92800  4520 ?        Ss   14:10   0:00 sshd: ubuntu [p
ubuntu    1794  0.0  0.4  45148  4424 ?        Ss   14:10   0:00 /lib/systemd/sy
ubuntu    1796  0.0  0.1  61120  1816 ?        S    14:10   0:00 (sd-pam)
ubuntu    1854  0.0  0.3  92800  3652 ?        S    14:10   0:00 sshd: ubuntu@pt
ubuntu    1855  0.0  0.4  21416  4488 pts/0    Ss   14:10   0:00 -bash
root      2062  0.0  0.0      0     0 ?        S    14:15   0:00 [kworker/u30:2]
root      9484  0.0  0.0      0     0 ?        S    14:20   0:00 [kworker/0:2]
root      9489  0.0  0.0      0     0 ?        S    14:20   0:00 [kworker/0:3]
root      9491  0.0  0.1 124980  1424 ?        Ss   14:20   0:00 nginx: master p
www-data  9492  0.0  0.3 125340  3132 ?        S    14:20   0:00 nginx: worker p
postgres 10852  0.0  2.3 293404 23848 ?        S    14:20   0:00 /usr/lib/postgr
postgres 10854  0.0  0.6 293536  7040 ?        Ss   14:20   0:00 postgres: check
postgres 10855  0.0  0.5 293404  5548 ?        Ss   14:20   0:00 postgres: write
postgres 10856  0.0  0.3 293404  4012 ?        Ss   14:20   0:00 postgres: wal w
postgres 10857  0.0  0.6 293840  6624 ?        Ss   14:20   0:00 postgres: autov
postgres 10858  0.0  0.3 148388  3184 ?        Ss   14:20   0:00 postgres: stats
ubuntu   11228  0.0  0.3  36084  3308 pts/0    R+   14:44   0:00 ps aux
```

- Django設定
  - `sudo -H pip3 install --upgrade pip`
  - `sudo -H pip3 install virtualenv` //仮想環境作るモジュールをインストール
  - `pwd`    ///home/ubuntu
  - `virtualenv py36` //仮想環境を作成
  - `ls py36/bin`    //アクティベート用のファイルを確認
  - `source py36/bin/activate`    //仮想環境を起動
  - `pip install django gunicorn psycopg2`
  - `pip install pillow`  //画像を扱うpillowをインストール
  - `python3 manage.py createsuperuser` //user作成

