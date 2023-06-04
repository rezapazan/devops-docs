***Table of Contents:***

- [Cloud Architectures](#cloud-architectures)
  - [SaaS (Software as a Service)](#saas-software-as-a-service)
    - [Multi-Tenancy](#multi-tenancy)
    - [Pros \& Cons](#pros--cons)
  - [PaaS (Platform as a Service)](#paas-platform-as-a-service)
    - [Pros \& Cons](#pros--cons-1)
  - [IaaS (Infrastructure as a Service)](#iaas-infrastructure-as-a-service)
    - [Pros \& Cons](#pros--cons-2)


# Cloud Architectures

In this doc, we are going to discuss 3 famous cloud architecture models:
 - SaaS
 - PaaS
 - IaaS

as IaaS is a model for lower layers of the architecture.

## SaaS (Software as a Service)

Software as a service (SaaS) is a software distribution model in which a cloud provider hosts applications and makes them available to end users over the internet.

SaaS works through the cloud delivery model. A software provider will either host the application and related data using its own servers, databases, networking and computing resources, or it may be an independent software vendor (ISV) that contracts a cloud provider to host the application in the provider's data center. The application will be accessible to any device with a network connection. SaaS applications are typically accessed via web browsers.

As a result, companies using SaaS applications are not tasked with the setup and maintenance of the software. Users simply pay a subscription fee to gain access to the software, which is a ready-made solution. The application's source code is the same for all customers, and when new features or functionalities are released, they are rolled out to all customers.

**Note:** the customer's data for each model may be stored locally, in the cloud or both locally and in the cloud.

### Multi-Tenancy

The typical **multi-tenant** architecture of SaaS applications means the cloud service provider can manage maintenance, updates and bug fixes faster, easier and more efficiently. Rather than having to implement changes in multiple instances, engineers can make necessary changes for all customers by maintaining the one, shared instance.

**Note:** In cloud computing, multitenancy means that multiple customers of a cloud vendor are using the same computing resources. Despite the fact that they share resources, cloud customers are not aware of each other, and their data is kept totally separate.

### Pros & Cons

**Pros:**
- Flexible payments
- Scalable usage
- Automatic updates
- Accessibility and persistence
- Customization

**Cons:**
- Issues beyond customer control
- Customers lose control over versioning
- Difficulty switching vendors
- Security

## PaaS (Platform as a Service)

Platform as a service (PaaS) is a cloud computing model where a third-party provider delivers hardware and software tools to users over the internet. Usually, these tools are needed for application development. A PaaS provider hosts the hardware and software on its own infrastructure. As a result, PaaS frees developers from having to install in-house hardware and software to develop or run a new application.

PaaS provides a framework of resources for an organization's in-house developers. This hosted platform enables developers to create customized applications. The vendor manages the data center resources that support the tools. Customer organizations using PaaS services do not have to manage their OSes, but must manage applications and data use.

As mentioned above, PaaS does not replace a company's entire IT infrastructure for software development. It is provided through a cloud service provider's hosted infrastructure. Users most frequently access the offerings through a web browser.

### Pros & Cons

PaaS shifts the responsibility for providing, managing and updating key tools from the internal IT team to the outside PaaS provider.

PaaS architectures keep their underlying infrastructure hidden from developers and other users. As a result, the model is similar to serverless computing and function-as-a-service architectures -- meaning the cloud service provider manages and runs the server, as well as controlling the distribution of resources.

In terms of disadvantages, however, service availability or resilience can be a concern with PaaS. If a provider experiences a service outage or other infrastructure disruption, this can adversely affect customers and result in costly lapses of productivity. However, PaaS providers will normally offer and support relatively high uptimes.

Vendor lock-in is another common concern because users cannot easily migrate many of the services and data from one PaaS platform to another competing PaaS platform. Users must evaluate the business risks of service downtime and vendor lock-in when they select a PaaS provider.

## IaaS (Infrastructure as a Service)

Infrastructure as a service (IaaS) is a form of cloud computing that provides virtualized computing resources over the internet.

In an IaaS service model, a cloud provider hosts the infrastructure components that are traditionally present in an on-premises data center. This includes servers, storage and networking hardware, as well as the virtualization or hypervisor layer.

IaaS is used by companies that want to outsource their data center and computer resources to a cloud provider. IaaS providers host infrastructure components such as servers, storage, networking hardware and virtualization resources. Customer organizations using IaaS services must still manage their data use, applications and operating systems.

IaaS customers access resources and services through a wide area network (WAN), such as the internet, and can use the cloud provider's services to install the remaining elements of an application stack. For example, the user can log in to the IaaS platform to create virtual machines (VMs); install operating systems in each VM; deploy middleware, such as databases; create storage buckets for workloads and backups; and install the enterprise workload into that VM. Customers can then use the provider's services to track costs, monitor performance, balance network traffic, troubleshoot application issues and manage disaster recovery.

### Pros & Cons

Organizations choose IaaS because it is often easier, faster and more cost-efficient to operate a workload without having to buy, manage and support the underlying infrastructure.

Insight is another common problem for IaaS users. Because IaaS providers own the infrastructure, the details of their infrastructure configuration and performance are rarely transparent to IaaS users. This lack of transparency can make systems management and monitoring more difficult for users.

IaaS users are also concerned about service resilience. The workload's availability and performance are highly dependent on the provider. If an IaaS provider experiences network bottlenecks or any form of internal or external downtime, the users' workloads will be affected. In addition, because IaaS is a multi-tenant architecture, the noisy neighbor issue can negatively impact users' workloads.