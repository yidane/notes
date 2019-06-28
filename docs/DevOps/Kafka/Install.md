# [Install kafka](https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-centos-7)

## Install [OpenJDK 8](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-centos-and-fedora#install-openjdk-8)

* Install OpenJDK 8 JRE

```bash
sudo yum install java-1.8.0-openjdk
```

* Install OpenJDK 8 JDK

```bash
sudo yum install java-1.8.0-openjdk-devel
```

## Creating a User for Kafka

```bash
sudo useradd kafka -m
```

> The -m flag ensures that a home directory will be created for the user. This home directory, /home/kafka, will act as our workspace directory for executing commands in the sections below.

```bash
sudo passwd kafka #meizhukafka
sudo usermod -aG wheel kafka
```

> Add the kafka user to the wheel group with the adduser command, so that it has the privileges required to install Kafka's dependencies:

## Downloading and Extracting the Kafka Binaries

```bash
su -l kafka
```

>login into kafka

```bash
mkdir ~/Downloads
curl "https://www.apache.org/dist/kafka/2.1.1/kafka_2.11-2.1.1.tgz" -o ~/Downloads/kafka.tgz
```

>Create directory of Downloads and download kafka into it.

```bash
mkdir ~/kafka && cd ~/kafka
tar -xvzf ~/Downloads/kafka.tgz --strip 1
```

>Create directory kafka and extract kafka into it.  
>
>We specify the --strip 1 flag to ensure that the archive's contents are extracted in ~/kafka/ itself and not in another directory (such as ~/kafka/kafka_2.11-2.1.1/) inside of it

## Configuring the Kafka Server

> Kafka's default behavior will not allow us to delete a topic, the category, group, or feed name to which messages can be published. To modify this, let's edit the configuration file.
>
> Kafka's configuration options are specified in server.properties. Open this file with vi or your favorite editor:

```bash
vi ~/kafka/config/server.properties
```

> Let's add a setting that will allow us to delete Kafka topics. Press i to insert text, and add the following to the bottom of the file:

```bash
delete.topic.enable = true
```

>and then save server.properties

## Creating Systemd Unit Files and Starting the Kafka Server

* Create the unit file for zookeeper

```bash
sudo vi /etc/systemd/system/zookeeper.service
```

>Enter the following unit definition into the file:

```bash
[Unit]
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
User=kafka
ExecStart=/home/kafka/kafka/bin/zookeeper-server-start.sh /home/kafka/kafka/config/zookeeper.properties
ExecStop=/home/kafka/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```

> The [Unit] section specifies that Zookeeper requires networking and the filesystem to be ready before it can start.
>
> The [Service] section specifies that systemd should use the zookeeper-server-start.sh and zookeeper-server-stop.sh shell files for starting and stopping the service. It also specifies that Zookeeper should be restarted automatically if it exits abnormally.
>
> Save and close the file when you are finished editing.

* Create the systemd service file for kafka:

```bash
sudo vi /etc/systemd/system/kafka.service
```

Enter the following unit definition into the file:

```bash
[Unit]
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/server.properties > /home/kafka/kafka/kafka.log 2>&1'
ExecStop=/home/kafka/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```

>Save and close the file when you are finished editing

* Start Kafka

```bash
sudo systemctl start kafka
journalctl -u kafka #check the journal logs for the kafka unit if needed
```

* Enable kafka on server boot
  
```bash
sudo systemctl enable kafka
```

## Testing the Installation

> Let's publish and consume a "Hello World" message to make sure the Kafka server is behaving correctly. Publishing messages in Kafka requires:
>
> * A producer, which enables the publication of records and data to topics.
> * A consumer, which reads messages and data from topics.

* Create a topic named TutorialTopic

```bash
~/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic TutorialTopic

# you will recive this as follow
# Created topic "TutorialTopic".
```

* Publish the string "Hello, World" to the TutorialTopic topic

```bash
echo "Hello, World" | ~/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic TutorialTopic > /dev/null
```

* Consume from the TutorialTopic topic

```bash
~/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic TutorialTopic --from-beginning

#you will recive message from kakafa as follow
#Hello World
```

## Installing KafkaT (Optional)

> KafkaT is a tool from Airbnb that makes it easier for you to view details about your Kafka cluster and perform certain administrative tasks from the command line. Because it is a Ruby gem, you will need Ruby to use it. You will also need ruby-devel and build-related packages such as make and gcc to be able to build the other gems it depends on.

```bash
sudo yum install ruby ruby-devel make gcc patch
```

> You can now install KafkaT using the gem command

```bash
sudo gem install kafkat
```

> KafkaT uses .kafkatcfg as the configuration file to determine the installation and log directories of your Kafka server. It should also have an entry pointing KafkaT to your ZooKeeper instance.
>
>Create a new file called .kafkatcfg

```bash
vi ~/.kafkatcfg
```

> Add the following lines to specify the required information about your Kafka server and Zookeeper instance

```bash
{
  "kafka_path": "~/kafka",
  "log_path": "/tmp/kafka-logs",
  "zk_path": "localhost:2181"
}
```

> You are now ready to use KafkaT. For a start, here's how you would use it to view details about all Kafka partitions

```bash
kafkat partitions

#You will see the following output
Topic                 Partition   Leader      Replicas        ISRs    
TutorialTopic         0             0         [0]             [0]
__consumer_offsets    0             0         [0]                           [0]
...
...
```
