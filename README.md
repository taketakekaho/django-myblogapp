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
- Webサーバー設定
  - `which gumicorn` //Pythonを動かすアプリケーションサーバーであるGunicornがインストールされているかを確認
  - `gunicorn --bind 0.0.0.0:8000 myblogapp.wsgi` //8000ポート指定してmyblogapp/wsgi.pyを参照する
  - `deactivate` //仮想環境から抜ける
  - `ls /etc/systemd/system` //設定ファイル確認

```
[Unit]
Description=OpenBSD Secure Shell server
After=network.target auditd.service
ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
EnvironmentFile=-/etc/default/ssh
ExecStartPre=/usr/sbin/sshd -t
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
ExecReload=/usr/sbin/sshd -t
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartPreventExitStatus=255
Type=notify

[Install]
WantedBy=multi-user.target
Alias=sshd.service
```
  - `ls -la /etc/systemd/system` //root権限しか書き込み権限なし
```
total 80
drwxr-xr-x 15 root root 4096 May 23 21:49 .
drwxr-xr-x  5 root root 4096 May 24 06:43 ..
drwxr-xr-x  2 root root 4096 Apr  6 08:17 cloud-final.service.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:17 cloud-init.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:13 default.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:17 final.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:13 getty.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:16 graphical.target.wants
lrwxrwxrwx  1 root root   38 Apr  6 08:17 iscsi.service -> /lib/systemd/system/open-iscsi.service
drwxr-xr-x  2 root root 4096 May 23 21:49 multi-user.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:14 network-online.target.wants
drwxr-xr-x  2 root root 4096 Apr  6 08:17 open-vm-tools.service.requires
drwxr-xr-x  2 root root 4096 Apr  6 08:17 paths.target.wants
-rw-r--r--  1 root root  529 May 23 21:49 snap.amazon-ssm-agent.amazon-ssm-agent.service
-rw-r--r--  1 root root  263 May 23 14:08 snap-amazon\x2dssm\x2dagent-1068.mount
-rw-r--r--  1 root root  263 May 23 21:48 snap-amazon\x2dssm\x2dagent-1335.mount
-rw-r--r--  1 root root  227 May 23 14:08 snap-core-6673.mount
-rw-r--r--  1 root root  227 May 23 21:48 snap-core-6964.mount
drwxr-xr-x  2 root root 4096 Apr  6 08:17 sockets.target.wants
lrwxrwxrwx  1 root root   31 Apr  6 08:17 sshd.service -> /lib/systemd/system/ssh.service
drwxr-xr-x  2 root root 4096 Apr  6 08:17 sysinit.target.wants
lrwxrwxrwx  1 root root   35 Apr  6 08:14 syslog.service -> /lib/systemd/system/rsyslog.service
drwxr-xr-x  2 root root 4096 Apr  6 08:17 timers.target.wants
```

  - `sudo vi /etc/systemd/system/gumicorn.service` //以下を書き込み保存

```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/myblogapp
ExecStart=/home/ubuntu/py36/bin/gunicorn --access-logfile - --workers 3 --bind u
nix:/home/ubuntu/myblogapp/myblogapp.sock myblogapp.wsgi:application

[Install]
WantedBy=multi-user.target
```

  - `sudo systemctl start gunicorn`  //gunicorn.serviceファイルからエイリアスを作成

```
Created symlink from /etc/systemd/system/multi-user.target.wants/gunicorn.service to /etc/systemd/system/gunicorn.service.
```
  - `ls -la /etc/systemd/system/multi-user.target.wants` //gunicorn.serviceがあることの確認

```
drwxr-xr-x  2 root root 4096 May 29 16:47 .
drwxr-xr-x 15 root root 4096 May 29 16:46 ..
lrwxrwxrwx  1 root root   31 Apr  6 08:17 atd.service -> /lib/systemd/system/atd.service
lrwxrwxrwx  1 root root   32 Apr  6 08:13 cron.service -> /lib/systemd/system/cron.service
lrwxrwxrwx  1 root root   36 May 29 16:47 gunicorn.service -> /etc/systemd/system/gunicorn.service
lrwxrwxrwx  1 root root   33 Apr  6 08:17 lxcfs.service -> /lib/systemd/system/lxcfs.service
lrwxrwxrwx  1 root root   42 Apr  6 08:17 lxd-containers.service -> /lib/systemd/system/lxd-containers.service
lrwxrwxrwx  1 root root   38 Apr  6 08:14 networking.service -> /lib/systemd/system/networking.service
lrwxrwxrwx  1 root root   33 May 23 14:20 nginx.service -> /lib/systemd/system/nginx.service
lrwxrwxrwx  1 root root   41 Apr  6 08:17 open-vm-tools.service -> /lib/systemd/system/open-vm-tools.service
lrwxrwxrwx  1 root root   37 Apr  6 08:17 pollinate.service -> /lib/systemd/system/pollinate.service
lrwxrwxrwx  1 root root   38 May 23 14:20 postgresql.service -> /lib/systemd/system/postgresql.service
lrwxrwxrwx  1 root root   36 Apr  6 08:13 remote-fs.target -> /lib/systemd/system/remote-fs.target
lrwxrwxrwx  1 root root   35 Apr  6 08:14 rsyslog.service -> /lib/systemd/system/rsyslog.service
lrwxrwxrwx  1 root root   66 May 23 21:49 snap.amazon-ssm-agent.amazon-ssm-agent.service -> /etc/systemd/system/snap.amazon-ssm-agent.amazon-ssm-agent.service
lrwxrwxrwx  1 root root   58 May 23 14:08 snap-amazon\x2dssm\x2dagent-1068.mount -> /etc/systemd/system/snap-amazon\x2dssm\x2dagent-1068.mount
lrwxrwxrwx  1 root root   58 May 23 21:48 snap-amazon\x2dssm\x2dagent-1335.mount -> /etc/systemd/system/snap-amazon\x2dssm\x2dagent-1335.mount
lrwxrwxrwx  1 root root   40 May 23 14:08 snap-core-6673.mount -> /etc/systemd/system/snap-core-6673.mount
lrwxrwxrwx  1 root root   40 May 23 21:48 snap-core-6964.mount -> /etc/systemd/system/snap-core-6964.mount
lrwxrwxrwx  1 root root   44 Apr  6 08:17 snapd.autoimport.service -> /lib/systemd/system/snapd.autoimport.service
lrwxrwxrwx  1 root root   44 Apr  6 08:17 snapd.core-fixup.service -> /lib/systemd/system/snapd.core-fixup.service
lrwxrwxrwx  1 root root   40 Apr  6 08:17 snapd.seeded.service -> /lib/systemd/system/snapd.seeded.service
lrwxrwxrwx  1 root root   33 Apr  6 08:17 snapd.service -> /lib/systemd/system/snapd.service
lrwxrwxrwx  1 root root   31 Apr  6 08:17 ssh.service -> /lib/systemd/system/ssh.service
lrwxrwxrwx  1 root root   31 Apr  6 08:17 ufw.service -> /lib/systemd/system/ufw.service
lrwxrwxrwx  1 root root   47 Apr  6 08:17 unattended-upgrades.service -> /lib/systemd/system/unattended-upgrades.service
```
  - `ls -la /home/ubuntu/myblogapp` //myblogapp.sockがあることの確認

```
total 176
drwxr-xr-x 6 ubuntu ubuntu     4096 May 29 16:47 .
drwxr-xr-x 8 ubuntu ubuntu     4096 May 29 16:46 ..
-rw-r--r-- 1 ubuntu ubuntu   135168 Apr 14 12:44 db.sqlite3
drwxrwxr-x 8 ubuntu ubuntu     4096 May 29 16:10 .git
-rw-rw-r-- 1 ubuntu ubuntu       58 May 23 16:20 .gitignore
-rw-r--r-- 1 ubuntu ubuntu      556 Mar  3 14:55 manage.py
drwxr-xr-x 2 ubuntu ubuntu     4096 May 29 16:05 media
drwxr-xr-x 3 ubuntu ubuntu     4096 May 23 17:18 myblogapp
srwxrwxrwx 1 ubuntu www-data      0 May 29 16:47 myblogapp.sock
drwxr-xr-x 6 ubuntu ubuntu     4096 May 23 15:37 posts
-rw-rw-r-- 1 ubuntu ubuntu    10378 May 29 16:08 README.md
```

  - `sudo systemctl status gunicorn` //gunicornのステータスを表示

```
● gunicorn.service - gunicorn daemon
   Loaded: loaded (/etc/systemd/system/gunicorn.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-05-29 16:47:18 UTC; 12min ago
 Main PID: 25642 (gunicorn)
    Tasks: 4
   Memory: 90.8M
      CPU: 1.056s
   CGroup: /system.slice/gunicorn.service
           tq25642 /home/ubuntu/py36/bin/python3 /home/ubuntu/py36/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/ubuntu/myblo
           tq25648 /home/ubuntu/py36/bin/python3 /home/ubuntu/py36/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/ubuntu/myblo
           tq25649 /home/ubuntu/py36/bin/python3 /home/ubuntu/py36/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/ubuntu/myblo
           mq25651 /home/ubuntu/py36/bin/python3 /home/ubuntu/py36/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/ubuntu/myblo

May 29 16:47:18 ip-172-31-38-174 systemd[1]: Started gunicorn daemon.
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Starting gunicorn 19.9.0
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Listening at: unix:/home/ubuntu/myblogapp/myblog
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Using worker: sync
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25648] [INFO] Booting worker with pid: 25648
May 29 16:47:19 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:19 +0000] [25649] [INFO] Booting worker with pid: 25649
May 29 16:47:19 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:19 +0000] [25651] [INFO] Booting worker with pid: 25651
```

  - `sudo journalctl -u gunicorn` //gunicornのログを確認

```
-- Logs begin at Sat 2019-05-25 19:17:01 UTC, end at Wed 2019-05-29 17:05:52 UTC. --
May 29 16:47:18 ip-172-31-38-174 systemd[1]: Started gunicorn daemon.
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Starting gunicorn 19.9.0
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Listening at: unix:/home/ubuntu/myblogapp/myblog
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25642] [INFO] Using worker: sync
May 29 16:47:18 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:18 +0000] [25648] [INFO] Booting worker with pid: 25648
May 29 16:47:19 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:19 +0000] [25649] [INFO] Booting worker with pid: 25649
May 29 16:47:19 ip-172-31-38-174 gunicorn[25642]: [2019-05-29 16:47:19 +0000] [25651] [INFO] Booting worker with pid: 25651
```



`` //