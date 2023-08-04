# vpc-peering-with-gcp-in-multiple-region
What is VPC?

The word “VPC” stands for “Virtual Private Cloud.” Many cloud service providers, such as Microsoft Azure, Amazon Web Services (AWS), and Google Cloud Platform (GCP), offer this cloud computing service. VPC enables users to create a virtual network that is logically segregated inside a public cloud architecture.

Simply explained, a VPC enables users to create and use their own network environment in the cloud. Setting up routing tables, defining IP ranges, creating subnets, and configuring network gateways are required for this. Using a VPC, users may deploy their resources (such virtual machines, databases, and containers) in a private, secure, and isolated network region while keeping access to the internet and other services.

One of the vital advantages of VPC is it isolation which restrict other users to unauthorized access.

What we are going to make?

<img width="478" alt="Screenshot 2023-08-04 030002" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/c0ef103a-d18c-4b78-be82-b74e44c53600">

Lets start:

At first we have to create a two vpc with two subnet in two different region. One is us central and another is us east. Assign cider 172.20.1.0/24 for one subnet and assign 172.20.2.0/24 for the another vpc subnet.


<img width="761" alt="vpc" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/2db9d21f-05a7-494c-9c12-d0cec2f0ca9c">

Firewall rules:

At vpc-1 we will ssh and ping vpc-2 vm so we need to allow only two firewall rules

<img width="755" alt="firewall-vpc-1" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/cced0f94-7b44-4f85-9bb1-70fe1c0fc823">

For vpc-2 vm we need to allow http because we will make a curl request from vpc-1 vm . Also need to allow ssh and icmp.

<img width="744" alt="firewall-vpc-2" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/5866e633-aa86-4654-8aca-a5d33c87550e">

Now we have to launch two vm in two vpc. Here instance-1 is in vpc-1 and instance-2 is in vpc-2.

<img width="767" alt="vm" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/f0eab090-1009-4c4d-90c2-05c432258a47">

Lets ssh instance-1 and try to ping instance-2 which is in different vpc

<img width="953" alt="ping-not-working" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/f142ef4b-07ca-44e3-bf68-29c2f6cdb8d6">

We are not getting any response. It is as expected as two vms are in two different vpc.

If we want to communicate between these two vm we have to do vpc peering. Let’s try vpc peering and see the result.


<img width="753" alt="vpc-peering" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/9ec76ac3-10fd-4a8a-a9f3-0a1908afa8a7">


Our vpc peering is done. Now let’s try to ping instance-2 from instance-1.

<img width="946" alt="ping-after-vpc-peering" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/207ec6fb-8502-4e74-856e-97ad4fd9eef2">

See now it is working though two vms are in two diffrent vpc. It is because we have applied vpc peering.

Now let’s spin up a nginx docker container in instance-2 and make a curl request from instance-1.

Follow these steps to spin up a nginx docker container.

```
1.sudo apt install docker.io
2.sudo systemctl start docker
3.docker pull nginx
4.docker run -d --name my_nginx_container -p 80:80 nginx
```

Let make http request from instance-1 to instance-2 at 80 port as it is open for http request in our firewall rules.

<img width="935" alt="curl-from-another" src="https://github.com/nobelrakib/vpc-peering-with-gcp-in-multiple-region/assets/53372696/87b15b3f-65a5-4404-8226-d62834e57de4">

See http request is getting response!!!






