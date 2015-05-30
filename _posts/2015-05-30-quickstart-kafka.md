---
layout: post
title : QuickStart Kafka
date:   2015-05-30 21:45:56
categories: memo
---

## 参考

- [https://github.com/apache/kafka](https://github.com/apache/kafka)
- [http://kafka.apache.org/documentation.html#quickstart](https://github.com/apache/kafka)

## 手順

# 動作イメージ


# 動作環境

{% highlight sh %}
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.2 LTS"
{% endhighlight sh %}

# 準備

{% highlight sh %}
sudo apt-get install openjdk-7-jdk
wget https://services.gradle.org/distributions/gradle-2.4-all.zip
unzip gradle-2.4-all.zip
wget http://ftp.kddilabs.jp/infosystems/apache/kafka/0.8.2.1/kafka-0.8.2.1-src.tgz
{% endhighlight sh %}

# bootstrap

{% highlight sh %}
tar zxvf kafka-0.8.2.1-src.tgz
cd kafka-0.8.2.1-src
sudo ~/gradle-2.4/bin/gradle
{% endhighlight sh %}

`apt-get` でインストールした gradle だと以下のようなエラーが出てしまうので注意。

{% highlight sh %}
FAILURE: Build failed with an exception.

* Where:
Script '/home/vagrant/kafka/kafka-0.8.2.1-src/gradle/license.gradle' line: 2

* What went wrong:
A problem occurred evaluating script.
> Could not find method create() for arguments [downloadLicenses, class nl.javadude.gradle.plugins.license.DownloadLicenses] on task set.

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 7.491 secs
{% endhighlight sh %}

# jar build

{% highlight sh %}
sudo ./gradlew jar
{% endhighlight sh %}

# srcJar build

{% highlight sh %}
sudo ./gradlew srcJar
{% endhighlight sh %}

# zookeeper と kafka の起動

- kafka の起動には zookeeper が起動しておく必要がある

{% highlight sh %}
# zookeeper の起動
sudo bin/zookeeper-server-start.sh config/zookeeper.properties &
# kafka の起動
sudo bin/kafka-server-start.sh config/server.properties &
{% endhighlight sh %}

# topic の作成と確認

- topic の作成

{% highlight sh %}
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
{% endhighlight sh %}

以下のように出力される。

{% highlight sh %}
/bin/bash: warning: setlocale: LC_ALL: cannot change locale (ja_JP.UTF-8)
/bin/bash: warning: setlocale: LC_ALL: cannot change locale (ja_JP.UTF-8)
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/vagrant/kafka/kafka-0.8.2.1-src/core/build/dependant-libs-2.10.4/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/vagrant/kafka/kafka-0.8.2.1-src/core/build/dependant-libs-2.10.4/slf4j-log4j12-1.7.6.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
[2015-05-12 03:53:44,011] INFO Accepted socket connection from /127.0.0.1:42571 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2015-05-12 03:53:44,017] INFO Client attempting to establish new session at /127.0.0.1:42571 (org.apache.zookeeper.server.ZooKeeperServer)
[2015-05-12 03:53:44,019] INFO Established session 0x14d46400c460001 with negotiated timeout 30000 for client /127.0.0.1:42571 (org.apache.zookeeper.server.ZooKeeperServer)
[2015-05-12 03:53:44,292] INFO Got user-level KeeperException when processing sessionid:0x14d46400c460001 type:setData cxid:0x3 zxid:0x1a txntype:-1 reqpath:n/a Error Path:/config/topics/test Error:KeeperErrorCode = NoNode for /config/topics/test (org.apache.zookeeper.server.PrepRequestProcessor)
[2015-05-12 03:53:44,316] INFO Got user-level KeeperException when processing sessionid:0x14d46400c460001 type:create cxid:0x4 zxid:0x1b txntype:-1 reqpath:n/a Error Path:/config/topics Error:KeeperErrorCode = NodeExists for /config/topics (org.apache.zookeeper.server.PrepRequestProcessor)
Created topic "test".
[2015-05-12 03:53:44,343] INFO Processed session termination for sessionid: 0x14d46400c460001 (org.apache.zookeeper.server.PrepRequestProcessor)
[2015-05-12 03:53:44,346] INFO Closed socket connection for client /127.0.0.1:42571 which had sessionid 0x14d46400c460001 (org.apache.zookeeper.server.NIOServerCnxn)
[2015-05-12 03:53:44,395] INFO Got user-level KeeperException when processing sessionid:0x14d46400c460000 type:create cxid:0x2b zxid:0x1f txntype:-1 reqpath:n/a Error Path:/brokers/topics/test/partitions/0 Error:KeeperErrorCode = NoNode for /brokers/topics/test/partitions/0 (org.apache.zookeeper.server.PrepRequestProcessor)
[2015-05-12 03:53:44,397] INFO Got user-level KeeperException when processing sessionid:0x14d46400c460000 type:create cxid:0x2c zxid:0x20 txntype:-1 reqpath:n/a Error Path:/brokers/topics/test/partitions Error:KeeperErrorCode = NoNode for /brokers/topics/test/partitions (org.apache.zookeeper.server.PrepRequestProcessor)
[2015-05-12 03:53:44,467] INFO [ReplicaFetcherManager on broker 0] Removed fetcher for partitions [test,0] (kafka.server.ReplicaFetcherManager)
[2015-05-12 03:53:44,498] INFO Completed load of log test-0 with log end offset 0 (kafka.log.Log)
[2015-05-12 03:53:44,510] INFO Created log for partition [test,0] in /tmp/kafka-logs with properties {segment.index.bytes -> 10485760, file.delete.delay.ms -> 60000, segment.bytes -> 1073741824, flush.ms -> 9223372036854775807, delete.retention.ms -> 86400000, index.interval.bytes -> 4096, retention.bytes -> -1, min.insync.replicas -> 1, cleanup.policy -> delete, unclean.leader.election.enable -> true, segment.ms -> 604800000, max.message.bytes -> 1000012, flush.messages -> 9223372036854775807, min.cleanable.dirty.ratio -> 0.5, retention.ms -> 604800000, segment.jitter.ms -> 0}. (kafka.log.LogManager)
[2015-05-12 03:53:44,512] WARN Partition [test,0] on broker 0: No checkpointed highwatermark is found for partition [test,0] (kafka.cluster.Partition)
{% endhighlight sh %}

- topic の確認

{% highlight sh %}
bin/kafka-topics.sh --list --zookeeper localhost:2181
{% endhighlight sh %}

以下のように出力される。

{% highlight sh %}
/bin/bash: warning: setlocale: LC_ALL: cannot change locale (ja_JP.UTF-8)
/bin/bash: warning: setlocale: LC_ALL: cannot change locale (ja_JP.UTF-8)
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/vagrant/kafka/kafka-0.8.2.1-src/core/build/dependant-libs-2.10.4/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/vagrant/kafka/kafka-0.8.2.1-src/core/build/dependant-libs-2.10.4/slf4j-log4j12-1.7.6.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
[2015-05-12 03:54:58,953] INFO Accepted socket connection from /127.0.0.1:42572 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2015-05-12 03:54:58,959] INFO Client attempting to establish new session at /127.0.0.1:42572 (org.apache.zookeeper.server.ZooKeeperServer)
[2015-05-12 03:54:58,960] INFO Established session 0x14d46400c460002 with negotiated timeout 30000 for client /127.0.0.1:42572 (org.apache.zookeeper.server.ZooKeeperServer)
test
[2015-05-12 03:54:59,047] INFO Processed session termination for sessionid: 0x14d46400c460002 (org.apache.zookeeper.server.PrepRequestProcessor)
[2015-05-12 03:54:59,050] INFO Closed socket connection for client /127.0.0.1:42572 which had sessionid 0x14d46400c460002 (org.apache.zookeeper.server.NIOServerCnxn)
{% endhighlight sh %}

# publisher の起動とメッセージの送信

# consumer の起動とメッセージの受信

## 動かしてから Kafka について

# そもそも Kafka とは

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
