# Sports Industry Digitalization Project

## Digitalize and analyze lift-ride data from resorts globally.


The Sports Industry Digitalization Project is a comprehensive initiative focused on creating a distributed system tailored to the unique needs of the sports industry. The primary objective is to digitalize and analyze lift-ride data obtained from multiple resorts across different geographical locations.

### Key Features

1. **Data Aggregation:**
   - The project involves the seamless aggregation of lift-ride data from resorts spread geographically.

2. **Data Analysis:**
   - Utilization of collected data for in-depth analysis, including the identification of frequently used lifts and the skiers who engage with them the most.

3. **Server-Side API:**
   - Implementation of a robust server-side API to handle incoming data and facilitate seamless communication between clients and the server.

4. **Multi-threaded Java Client:**
   - Construction of a high-performance, multithreaded Java client responsible for generating approximately 10,000 POST requests to the server.

5. **Error Handling:**
   - Incorporation of effective error-handling mechanisms in the Java client, allowing requests to be retried up to five times before being classified as failed.

6. **Performance Profiling:**
   - Detailed performance profiling, including the calculation of metrics such as mean and median response times, throughput, and p99 response time.

## Project Components

- **Server-Side API:** Handles incoming data, ensures data integrity and facilitates communication with clients.

- **Multi-threaded Java Client:** Generates a substantial volume of POST requests, incorporating error-handling mechanisms for robustness.

## Keywords

- Client-Server Architecture
- Multi-threaded Java Client
- HTTP Protocol
- Error Handling
- Concurrency

Feel free to explore and contribute to the advancement of digitalization in the sports industry with this exciting project!


#### Create VM Instance on Oracle Cloud

To use the Oracle Cloud, you will need to register a free account. And select your home region (to me, here is the Canada Southeast (Montreal) - Home RegionRegion Identifier: ca-montreal-1).

Then you go to the Launch resources, and select **Create a VM instance**:

![](img/launch.png)

Configure your, Image and Shape:

![](img/shape.png)

For free tier users, you have the `Always Free-eligible` on shape Ampere with 4 available CPUs and 24 GB available RAMs.

Please check: https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#freetier_topic_Always_Free_Resources_Infrastructure

You can allocate them into several instances, or you can put them all into one instance, as the figure shows.

Then you set up your networks. You can **Create new virtual cloud network** or use the exixting one if you have. Change the name you like and leave others to stay default if you don't know what to do.

![](img/network.png)

Add SSH Keys. You can use the one that oracle created for you, as the figure shows. And download the keys:

![](img/ssh.png)

Configure your volume size, 200 GB total for free tier users.

![](img/bootvolume.png)

Configure advanced options if you need them:

![](img/adv-options.png)

And finish the creation.

Once the instance is created, you will see your instance's public IP address on the top right:

![image-20230128143324154](img/instance-details.png)

#### SSH Key Setup

Go to your user's home folder, and copy and paste the downloaded private and public keys to the `.ssh` folder.

```bash
# limit the access of the private key
chmod 400 ~/.ssh/your_private_key.key
```

And edit the config file under the `.ssh`:

![image-20230128144352939](img/ssh-conf.png)

#### Login

You can refer to: https://docs.oracle.com/en/cloud/cloud-at-customer/occ-get-started/log-vm-using-ssh.html

Make sure you have the ssh install in your environment.

```bash
ssh <public_ip_address>
```

#### VM Environment Setup

```bash
sudo yum update
```

##### Open JDK11

https://access.redhat.com/documentation/en-us/openjdk/11/html/installing_and_using_openjdk_11_on_rhel/installing-openjdk11-on-rhel8

```bash
sudo yum install java-11-openjdk
```

##### Maven 3.8

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.8.7/binaries/apache-maven-3.8.7-bin.tar.gz
```

and follow: https://maven.apache.org/install.html

##### Network & Firewall Settings

If you want to expose the `8080` port to the public:

1. Go to the instance detail page:

   1. Click your `subnet-1` on the right middle:

      ![image-20230128150259840](img/subnet.png)

   2. Click `Default Security List for vcn-1`:

      ![image-20230128150511712](img/vcn-list.png)

   3. Add Ingress Rules for port range 8080 ~ 10080 (what ever you need)

      ![image-20230128150930978](img/port.png)

2. Open instance's firewall ports, refer to:

   https://docs.fedoraproject.org/en-US/quick-docs/firewalld/#opening-ports-firewalld-fedora

   and

   https://www.thegeekdiary.com/how-to-open-a-ports-in-centos-rhel-7/

   ```bash
   sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
   sudo firewall-cmd --reload
   ```

   Check if it opened:

   ```bash
   sudo firewall-cmd --list-ports
   ```

#### Deploy the Jetty Helloworld

```bash
git clone https://github.com/youyinnn/distributed_system_jetty_helloworld.git
```

```bash
cd ~/distributed_system_jetty_helloworld
```

##### If you use the `jetty:run` command

```bash
mvn jetty:run
```

![image-20230128151730452](img/jettyrun.png)

##### If you use the `exec:exec` command

```bash
mvn clean install exec:exec
```

Make sure you know where to configure the port and contextPath:

![image-20230128152548583](img/jetty-code-conf.png)

and visit:

![image-20230128152808118](img/visit2.png)
