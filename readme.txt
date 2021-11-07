Kubernetes has emerged as the de facto standard for orchestrating containers in data centers
as well as established in the public and private cloud. 


Also the so-called cloud native stack, which consists of many consists of various components, is based on the standard established by Kubernetes.
The term container describes a technology in which a host system runs applications in parallel
separate environments (Operating System Level Virtualization). In most cases
the monitoring, starting and stopping and further management of the containers is carried out
management software that takes on the so-called orchestration. Kubernetes sums it up
one or more related containers together in a pod. Since the focus of the
Block is on Kubernetes, is only used by pods and not by individual containers
spoken. The orchestration usually takes place in groups of jointly managed
Container hosts in one or more so-called clusters.
To operate the orchestration of pods and to manage them, several products have become
established that allow very large environments to be served. At its core, however, put
all on Kubernetes. When looking at the runtime that the processes on the
Container hosts operates, and orchestration, which runs the runtimes on multiple container hosts
controls to distinguish.
In addition to these two central components, Kubernetes is operated in the
in most cases still from a specialized infrastructure, to which z. B. Registries, code versioning
and storage, automation tools, management servers, storage systems or virtual
Networks belong.


The following terms are used in the block with this meaning:
• Application refers to a combination of several programs that together create a
fulfill a task
• Clusters are operating environments for containers with multiple nodes
• Container Network Interface (CNI) designates the virtual network in the cluster
• Containers are processes started from an image that run within operating system namespaces

Images are all software packages that comply with the Open Container Initiative (OCI), these
include both base images for your own images and those images that are unchanged in the
Use are
• Management Plane refers to all for the administration, i.e. orchestration of the nodes,
Applications used in runtimes and clusters
• Node refers to a server that is installed and optimized for the operation of runtime
• Pod denotes a collection of several containers that are within the same
Operating system namespaces are running
• Registry is the generic term for code management and the storage of images
• Runtime refers to the software that starts the software in the image as a container
1.2. Goal setting
The aim of this module is to protect information that is processed in Kubernetes clusters,
offered or transferred through it.
1.3. Delimitation and modeling
The block APP.4.4 Kubernetes is always together with the block SYS.1.6 Container
apply. It is not relevant which runtime is in use or which additional ones
Applications are part of the management plan.
The module contains basic requirements for the setup, operation and
Orchestration with Kubernetes as well as the specialized infrastructure that is necessary for operation.
The latter includes registries, CSI / CNI, nodes and automation software, as far as they are directly related to
interact with the cluster. The requirements for these applications mostly relate to the
Interfaces, but also contain requirements that affect the operation of these applications,
as long as they directly affect the security of the cluster. Other services common in the Kubernetes environment,
such as B. Automation for CI / CD pipelines and code management in e.g. B. Git, handles the block
not in depth.
The module comprehensively models a cluster. The applications of the management plane, services
for automation and the nodes are to be seen and treated here as a group.
Kubernetes clusters are used regardless of the purpose of the services or
Applications considered. Security requirements of possible services, such as B. Web server (APP.3.2
Web server) or server for groupware (see APP.5.1 Groupware) are the subject of their own modules.
2. Hazardous situation

3. Requirements
The specific requirements of the APP.4.4 Kubernetes module are listed below. Of the
ISB is responsible for ensuring that all requirements are met in accordance with the specified security concept
and be checked. The IM must always be involved in strategic decisions.
Further roles are also defined in the IT-Grundschutz Compendium. You should be busy
insofar as this is sensible and appropriate.
Responsibilities roles
Basically responsible for IT operations
No other responsibilities
APP.4.4 Kubernetes
Page 4 of 8
Exactly one role should basically be responsible. In addition, there can be others
Give responsibilities. If any of these additional roles take precedence for the fulfillment of a requirement
is responsible, then this role is put in square brackets after the heading of the requirement
listed. 


3.1. Basic requirements
The following requirements MUST be met primarily for this module.
APP.4.4.A1 Planning the separation of the applications (B)
Before commissioning, it MUST be planned how the applications operated in the pods and
their different test and production operating environments are separated. Based on the
Protection requirements of the applications MUST define the architecture of the namespaces,
Meta tags, clusters and networks adequately address the risks and whether virtualized servers are also included
and networks should be used.
The planning MUST contain regulations on network, CPU and permanent storage separation. the
Separation SHOULD also take into account the network zone concept and the protection requirements and rely on them
be coordinated.
Applications SHOULD each run in their own Kubernetes namespace that contains all programs
the application includes. Only applications with similar protection requirements and similar possible ones
Attack vectors SHOULD share a Kubernetes cluster.
APP.4.4.A2 Planning the automation with CI / CD (B)
If automation of the operation of applications in Kubernetes using CI / CD
takes place, this MAY ONLY take place after appropriate planning. The planning MUST be the entire
Life cycle from commissioning to decommissioning including operation, monitoring and updates
include. The role and rights concept MUST be part of the planning.
APP.4.4.A3 Identity and authorization management in Kubernetes (B)
Kubernetes and all other applications of the Management Plane HAVE to do each action one
Authenticate the user or, in automated operation, a corresponding software and
authorize, regardless of whether the actions are carried out via a client, a web interface or a
corresponding interface (API) takes place. Administrative actions MUST NOT be anonymous
take place.
Each user MUST ONLY receive the absolutely necessary rights. Permissions without
Restrictions MUST be issued very restrictively.
Only a small group of people SHOULD be authorized to automate processes
define. Only selected administrators SHOULD have the right to share in Kubernetes
Create or change for permanent storage (Persistent Volumes).
APP.4.4.A4 Separation of pods (B)
The operating system kernel of the nodes MUST have isolation mechanisms to restrict
Visibility and resource usage of the pods among each other (see Linux namespaces and
cgroups). The separation MUST include at least process IDs, inter-process communication,
Include user IDs, file system and network including host name.
APP.4.4.A5 Data backup in the cluster (B)
The cluster MUST be backed up. The data backup MUST include:
• Fixed storage (Persistent Volumes),
• Configuration files from Kubernetes and the other programs of the Management Plane,
APP.4.4 Kubernetes
Page 5 of 8
• the current state of the Kubernetes cluster including the extensions,
• databases of the configuration, namely here etcd,
• All infrastructure applications that are required to operate the cluster and those in it
Services are necessary and
• the data management of the code and image registries.
Snapshots for the operation of the applications SHOULD also be considered. Snapshots
MUST NOT replace the backup.

3.2. Standard requirements
Together with the basic requirements, the following requirements correspond to the state of the art
Technology for this building block. In principle, they SHOULD be fulfilled.
APP.4.4.A6 Initialization of pods (S)
If an initialization z. B. an application, this SHOULD
Initialization takes place in a separate init container. It SHOULD be ensured that the
Initialization of all processes that are already running. Kubernetes SHOULD ONLY if successful
Start the initialization of the other containers.
APP.4.4.A7 Separation of the networks in Kubernetes (S)
The networks for the administration of the nodes, the management plan and the individual networks of the
Application services SHOULD be separated.
ONLY the network ports of the pods that are necessary for operation SHOULD be connected to the designated network ports
Networks are released. SHOULD if you have multiple applications on a Kubernetes cluster
initially all network connections between the namespaces are prohibited and only required
Network connections are permitted (whitelisting). The administration of the nodes, the runtime and
network ports required by Kubernetes including its extensions SHOULD ONLY be taken from the
Administration network and be reachable by pods that need them.
Only selected administrators SHOULD have permission in Kubernetes to manage the CNI and
Create or change rules for the network.
APP.4.4.A8 Securing configuration files with Kubernetes (S)
The configuration files of the Kubernetes cluster, including all extensions and applications
SHOULD be versioned and annotated.
Access rights to the administration software



3.3. Requirements for increased protection needs
Below are examples of requirements for this module that
go beyond the level of protection that corresponds to the state of the art. The suggestions
SHOULD be considered if there is an increased need for protection. The specific definition takes place in
As part of an individual risk analysis.
APP.4.4.A13 Automated auditing of the configuration (H)
There SHOULD be an automatic audit of the settings of the nodes, Kubernetes and the pods of the
Applications against a defined list of permitted settings and against standardized ones
Benchmarks.
Kubernetes SHOULD follow the rules established in the cluster by connecting suitable tools
push through.
APP.4.4.A14 Use of dedicated nodes (H)
In a Kubernetes cluster, the nodes SHOULD be assigned dedicated tasks and
only operate pods that are assigned to the respective task.
Bastion nodes SHOULD close all incoming and outgoing data connections to the applications
take over other networks.
Management nodes SHOULD run the management plane's pods and they SHOULD only run those
Take over the data connections of the management plane.
If used, storage nodes SHOULD only operate the pods of the storage services in the cluster.
APP.4.4 Kubernetes
Page 7 of 8
Kubernetes has emerged as the de facto standard for orchestrating containers in data centers
as well as established in the public and private cloud. Also for IoT and other use cases is
Kubernetes in use, with K3S, for example, there is an edition that is designed for very small servers such as
Single board computer is intended. Also the so-called cloud native stack, which consists of many
consists of various components, is based on the standard established by Kubernetes.
The term container describes a technology in which a host system runs applications in parallel
separate environments (Operating System Level Virtualization). In most cases
the monitoring, starting and stopping and further management of the containers is carried out
management software that takes on the so-called orchestration. Kubernetes sums it up
one or more related containers together in a pod. Since the focus of the
Block is on Kubernetes, is only used by pods and not by individual containers
spoken. The orchestration usually takes place in groups of jointly managed
Container hosts in one or more so-called clusters.
To operate the orchestration of pods and to manage them, several products have become
established that allow very large environments to be served. At its core, however, put
all on Kubernetes. When looking at the runtime that the processes on the
Container hosts operates, and orchestration, which runs the runtimes on multiple container hosts
controls to distinguish.
In addition to these two central components, Kubernetes is operated in the
in most cases still from a specialized infrastructure, to which z. B. Registries, code versioning
and storage, automation tools, management servers, storage systems or virtual
Networks belong.
The following terms are used in the block with this meaning:
• Application refers to a combination of several programs that together create a
fulfill a task
• Clusters are operating environments for containers with multiple nodes
• Container Network Interface (CNI) designates the virtual network in the cluster
• Containers are processes started from an image that run within operating system namespaces

Images are all software packages that comply with the Open Container Initiative (OCI), these
include both base images for your own images and those images that are unchanged in the
Use are
• Management Plane refers to all for the administration, i.e. orchestration of the nodes,
Applications used in runtimes and clusters
• Node refers to a server that is installed and optimized for the operation of runtime
• Pod denotes a collection of several containers that are within the same
Operating system namespaces are running
• Registry is the generic term for code management and the storage of images
• Runtime refers to the software that starts the software in the image as a container
1.2. Goal setting
The aim of this module is to protect information that is processed in Kubernetes clusters,
offered or transferred through it.
1.3. Delimitation and modeling
The block APP.4.4 Kubernetes is always together with the block SYS.1.6 Container
apply. It is not relevant which runtime is in use or which additional ones
Applications are part of the management plan.
The module contains basic requirements for the setup, operation and
Orchestration with Kubernetes as well as the specialized infrastructure that is necessary for operation.
The latter includes registries, CSI / CNI, nodes and automation software, as far as they are directly related to
interact with the cluster. The requirements for these applications mostly relate to the
Interfaces, but also contain requirements that affect the operation of these applications,
as long as they directly affect the security of the cluster. Other services common in the Kubernetes environment,
such as B. Automation for CI / CD pipelines and code management in e.g. B. Git, handles the block
not in depth.
The module comprehensively models a cluster. The applications of the management plane, services
for automation and the nodes are to be seen and treated here as a group.
Kubernetes clusters are used regardless of the purpose of the services or
Applications considered. Security requirements of possible services, such as B. Web server (APP.3.2
Web server) or server for groupware (see APP.5.1 Groupware) are the subject of their own modules.
2. Hazardous situation

825 / 5000
Translation results
3. Requirements
The specific requirements of the APP.4.4 Kubernetes module are listed below. Of the
ISB is responsible for ensuring that all requirements are met in accordance with the specified security concept
and be checked. The IM must always be involved in strategic decisions.
Further roles are also defined in the IT-Grundschutz Compendium. You should be busy
insofar as this is sensible and appropriate.
Responsibilities roles
Basically responsible for IT operations
No other responsibilities
APP.4.4 Kubernetes
Page 4 of 8
Exactly one role should basically be responsible. In addition, there can be others
Give responsibilities. If any of these additional roles take precedence for the fulfillment of a requirement
is responsible, then this role is put in square brackets after the heading of the requirement
listed. 


3.1. Basic requirements
The following requirements MUST be met primarily for this module.
APP.4.4.A1 Planning the separation of the applications (B)
Before commissioning, it MUST be planned how the applications operated in the pods and
their different test and production operating environments are separated. Based on the
Protection requirements of the applications MUST define the architecture of the namespaces,
Meta tags, clusters and networks adequately address the risks and whether virtualized servers are also included
and networks should be used.
The planning MUST contain regulations on network, CPU and permanent storage separation. the
Separation SHOULD also take into account the network zone concept and the protection requirements and rely on them
be coordinated.
Applications SHOULD each run in their own Kubernetes namespace that contains all programs
the application includes. Only applications with similar protection requirements and similar possible ones
Attack vectors SHOULD share a Kubernetes cluster.
APP.4.4.A2 Planning the automation with CI / CD (B)
If automation of the operation of applications in Kubernetes using CI / CD
takes place, this MAY ONLY take place after appropriate planning. The planning MUST be the entire
Life cycle from commissioning to decommissioning including operation, monitoring and updates
include. The role and rights concept MUST be part of the planning.
APP.4.4.A3 Identity and authorization management in Kubernetes (B)
Kubernetes and all other applications of the Management Plane HAVE to do each action one
Authenticate the user or, in automated operation, a corresponding software and
authorize, regardless of whether the actions are carried out via a client, a web interface or a
corresponding interface (API) takes place. Administrative actions MUST NOT be anonymous
take place.
Each user MUST ONLY receive the absolutely necessary rights. Permissions without
Restrictions MUST be issued very restrictively.
Only a small group of people SHOULD be authorized to automate processes
define. Only selected administrators SHOULD have the right to share in Kubernetes
Create or change for permanent storage (Persistent Volumes).
APP.4.4.A4 Separation of pods (B)
The operating system kernel of the nodes MUST have isolation mechanisms to restrict
Visibility and resource usage of the pods among each other (see Linux namespaces and
cgroups). The separation MUST include at least process IDs, inter-process communication,
Include user IDs, file system and network including host name.
APP.4.4.A5 Data backup in the cluster (B)
The cluster MUST be backed up. The data backup MUST include:
• Fixed storage (Persistent Volumes),
• Configuration files from Kubernetes and the other programs of the Management Plane,
APP.4.4 Kubernetes
Page 5 of 8
• the current state of the Kubernetes cluster including the extensions,
• databases of the configuration, namely here etcd,
• All infrastructure applications that are required to operate the cluster and those in it
Services are necessary and
• the data management of the code and image registries.
Snapshots for the operation of the applications SHOULD also be considered. Snapshots
MUST NOT replace the backup.

3.2. Standard requirements
Together with the basic requirements, the following requirements correspond to the state of the art
Technology for this building block. In principle, they SHOULD be fulfilled.
APP.4.4.A6 Initialization of pods (S)
If an initialization z. B. an application, this SHOULD
Initialization takes place in a separate init container. It SHOULD be ensured that the
Initialization of all processes that are already running. Kubernetes SHOULD ONLY if successful
Start the initialization of the other containers.
APP.4.4.A7 Separation of the networks in Kubernetes (S)
The networks for the administration of the nodes, the management plan and the individual networks of the
Application services SHOULD be separated.
ONLY the network ports of the pods that are necessary for operation SHOULD be connected to the designated network ports
Networks are released. SHOULD if you have multiple applications on a Kubernetes cluster
initially all network connections between the namespaces are prohibited and only required
Network connections are permitted (whitelisting). The administration of the nodes, the runtime and
network ports required by Kubernetes including its extensions SHOULD ONLY be taken from the
Administration network and be reachable by pods that need them.
Only selected administrators SHOULD have permission in Kubernetes to manage the CNI and
Create or change rules for the network.
APP.4.4.A8 Securing configuration files with Kubernetes (S)
The configuration files of the Kubernetes cluster, including all extensions and applications
SHOULD be versioned and annotated.
Access rights to the administration software



3.3. Requirements for increased protection needs
Below are examples of requirements for this module that
go beyond the level of protection that corresponds to the state of the art. The suggestions
SHOULD be considered if there is an increased need for protection. The specific definition takes place in
As part of an individual risk analysis.
APP.4.4.A13 Automated auditing of the configuration (H)
There SHOULD be an automatic audit of the settings of the nodes, Kubernetes and the pods of the
Applications against a defined list of permitted settings and against standardized ones
Benchmarks.
Kubernetes SHOULD follow the rules established in the cluster by connecting suitable tools
push through.
APP.4.4.A14 Use of dedicated nodes (H)
In a Kubernetes cluster, the nodes SHOULD be assigned dedicated tasks and
only operate pods that are assigned to the respective task.
Bastion nodes SHOULD close all incoming and outgoing data connections to the applications
take over other networks.
Management nodes SHOULD run the management plane's pods and they SHOULD only run those
Take over the data connections of the management plane.
If used, storage nodes SHOULD only operate the pods of the storage services in the cluster.
APP.4.4 Kubernetes
Page 7 of 8
APP.4.4.A15 Separation of applications on node and cluster level (H)
Applications with a very high protection requirement SHOULD each have their own Kubernetes cluster or
use dedicated nodes that are not available for other applications.
APP.4.4.A16 Use of operators (H)
The automation of operational tasks in operators SHOULD be particularly critical
Applications and the programs of the management plan are used.
APP.4.4.A17 Attestation of nodes (H)
Nodes SHOULD have a secure one that is cryptographically and, if possible, verified with a TPM
Send status message to the management plane. The management plane SHOULD ONLY be nodes in
join the cluster who have successfully demonstrated their integrity.
APP.4.4.A18 Use of micro-segmentation (H)
Even within a Kubernetes namespace, the pods SHOULD only use the necessary network ports
can communicate with each other. There SHOULD be rules within the CRF, all but one
the network connections necessary for operation within the Kubernetes namespace
prevent. These rules SHOULD precisely define the source and destination of the connections and for them
Use at least one of the following criteria: service name, metadata ("labels"), the Kubernetes
Service accounts or certificate-based authentication.
All criteria that serve as a designation for this connection SHOULD be secured in such a way that they
can only be changed by authorized persons and administrative services.
APP.4.4.A19 High availability of pods (H)
The operation SHOULD be structured in such a way that in the event of a failure of one location, the applications in the pods
either continue to run without interruption or in a short time at another location
can start. All necessary configuration files, images,
User data, network connections and other resources required for operation, including the
The hardware required for operation must already be available at this location. For the
Uninterrupted operation of the cluster SHOULD Kubernetes based on the pods of the applications
distribute location data of the nodes over several fire compartments in such a way that the failure of one
Fire compartment does not lead to the failure of the application.
APP.4.4.A20 Encrypted data storage for pods (H)
The file systems with the persistent data of the management plane (especially etcd here) and the
Application services SHOULD be encrypted.
APP.4.4.A21 Regular restart of pods (H)
If there is an increased risk of external influences and a very high protection requirement, pods SHOULD be
regularly stopped and restarted. No pod SHOULD be a


3.3. Requirements for increased protection needs
Below are examples of requirements for this module that
go beyond the level of protection that corresponds to the state of the art. The suggestions
SHOULD be considered if there is an increased need for protection. The specific definition takes place in
As part of an individual risk analysis.
APP.4.4.A13 Automated auditing of the configuration (H)
There SHOULD be an automatic audit of the settings of the nodes, Kubernetes and the pods of the
Applications against a defined list of permitted settings and against standardized ones
Benchmarks.
Kubernetes SHOULD follow the rules established in the cluster by connecting suitable tools
push through.
APP.4.4.A14 Use of dedicated nodes (H)
In a Kubernetes cluster, the nodes SHOULD be assigned dedicated tasks and
only operate pods that are assigned to the respective task.
Bastion nodes SHOULD close all incoming and outgoing data connections to the applications
take over other networks.
Management nodes SHOULD run the management plane's pods and they SHOULD only run those
Take over the data connections of the management plane.
If used, storage nodes SHOULD only operate the pods of the storage services in the cluster.
APP.4.4 Kubernetes
Page 7 of 8
APP.4.4.A15 Separation of applications on node and cluster level (H)
Applications with a very high protection requirement SHOULD each have their own Kubernetes cluster or
use dedicated nodes that are not available for other applications.
APP.4.4.A16 Use of operators (H)
The automation of operational tasks in operators SHOULD be particularly critical
Applications and the programs of the management plan are used.
APP.4.4.A17 Attestation of nodes (H)
Nodes SHOULD have a secure one that is cryptographically and, if possible, verified with a TPM
Send status message to the management plane. The management plane SHOULD ONLY be nodes in
join the cluster who have successfully demonstrated their integrity.
APP.4.4.A18 Use of micro-segmentation (H)
Even within a Kubernetes namespace, the pods SHOULD only use the necessary network ports
can communicate with each other. There SHOULD be rules within the CRF, all but one
the network connections necessary for operation within the Kubernetes namespace
prevent. These rules SHOULD precisely define the source and destination of the connections and for them
Use at least one of the following criteria: service name, metadata ("labels"), the Kubernetes
Service accounts or certificate-based authentication.
All criteria that serve as a designation for this connection SHOULD be secured in such a way that they
can only be changed by authorized persons and administrative services.
APP.4.4.A19 High availability of pods (H)
The operation SHOULD be structured in such a way that in the event of a failure of one location, the applications in the pods
either continue to run without interruption or in a short time at another location
can start. All necessary configuration files, images,
User data, network connections and other resources required for operation, including the
The hardware required for operation must already be available at this location. For the
Uninterrupted operation of the cluster SHOULD Kubernetes based on the pods of the applications
distribute location data of the nodes over several fire compartments in such a way that the failure of one
Fire compartment does not lead to the failure of the application.
APP.4.4.A20 Encrypted data storage for pods (H)
The file systems with the persistent data of the management plane (especially etcd here) and the
Application services SHOULD be encrypted.
APP.4.4.A21 Regular restart of pods (H)
If there is an increased risk of external influences and a very high protection requirement, pods SHOULD be
regularly stopped and restarted. No pod SHOULD run for more than 24 hours. Included
The availability of the applications in the pod SHOULD be ensured.



4. Additional information
4.1. useful information
Further information on hazards and safety measures in the area of containers
can be found in the following publications:
• NIST 800-190
https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf
APP.4.4 Kubernetes
Page 8 of 8
• CIS benchmark Kubernetes
https://www.cisecurity.org/benchmark/kubernetes/
• OCI - Open Container Initiative
https://www.opencontainers.org/
• CNCF - Cloud Native Computing Foundation
https://www.cncf.io/


5. Appendix: Cross-reference table to elementary
Hazards
Safety measures can be derived from every requirement (A) in this module. the
Implementation of these measures counteracts those elementary threats (G0) that are relevant for the
Topic or target object are relevant. In the cross reference table (KRT) for this module are everyone
The corresponding elementary hazards are assigned to the requirement.
The KRT can be used to determine which elementary hazards are caused by which requirements
are covered. The letters in the second column indicate which basic values ​​the
Information security should be protected by the requirement as a priority. These core values ​​are
Confidentiality (C) for confidentiality, Integrity (I) for integrity and Availability (A) for
Availability.
The following elementary threats are important for the APP.4.4 Kubernetes module:
G 0.14 Spying on information / espionage
G 0.19 Disclosure of information worthy of protection
G 0.20 Information or products from an unreliable source
G 0.21 Manipulation of hardware or software
G 0.23 Unauthorized entry into IT systems
G 0.25 Failure of devices or systems
G 0.27 Lack of resources
G 0.28 Software weaknesses or errors
G 0.30 Unauthorized use or administration of devices and systems
G 0.37 Denial of actions
G 0.39 malicious programs
G 0.45 Loss of data
G 0.46 Loss of integrity of information worthy of protection












APP.4.4.A16 Use of operators (H)
The automation of operational tasks in operators SHOULD be particularly critical
Applications and the programs of the management plan are used.
APP.4.4.A17 Attestation of nodes (H)
Nodes SHOULD have a secure one that is cryptographically and, if possible, verified with a TPM
Send status message to the management plane. The management plane SHOULD ONLY be nodes in
join the cluster who have successfully demonstrated their integrity.
APP.4.4.A18 Use of micro-segmentation (H)
Even within a Kubernetes namespace, the pods SHOULD only use the necessary network ports
can communicate with each other. There SHOULD be rules within the CRF, all but one
the network connections necessary for operation within the Kubernetes namespace
prevent. These rules SHOULD precisely define the source and destination of the connections and for them
Use at least one of the following criteria: service name, metadata ("labels"), the Kubernetes
Service accounts or certificate-based authentication.
All criteria that serve as a designation for this connection SHOULD be secured in such a way that they
can only be changed by authorized persons and administrative services.
APP.4.4.A19 High availability of pods (H)
The operation SHOULD be structured in such a way that in the event of a failure of one location, the applications in the pods
either continue to run without interruption or in a short time at another location
can start. All necessary configuration files, images,
User data, network connections and other resources required for operation, including the
The hardware required for operation must already be available at this location. For the
Uninterrupted operation of the cluster SHOULD Kubernetes based on the pods of the applications
distribute location data of the nodes over several fire compartments in such a way that the failure of one
Fire compartment does not lead to the failure of the application.
APP.4.4.A20 Encrypted data storage for pods (H)
The file systems with the persistent data of the management plane (especially etcd here) and the
Application services SHOULD be encrypted.
APP.4.4.A21 Regular restart of pods (H)
If there is an increased risk of external influences and a very high protection requirement, pods SHOULD be
regularly stopped and restarted. No pod SHOULD be a


3.3. Requirements for increased protection needs
Below are examples of requirements for this module that
go beyond the level of protection that corresponds to the state of the art. The suggestions
SHOULD be considered if there is an increased need for protection. The specific definition takes place in
As part of an individual risk analysis.
APP.4.4.A13 Automated auditing of the configuration (H)
There SHOULD be an automatic audit of the settings of the nodes, Kubernetes and the pods of the
Applications against a defined list of permitted settings and against standardized ones
Benchmarks.
Kubernetes SHOULD follow the rules established in the cluster by connecting suitable tools
push through.
APP.4.4.A14 Use of dedicated nodes (H)
In a Kubernetes cluster, the nodes SHOULD be assigned dedicated tasks and
only operate pods that are assigned to the respective task.
Bastion nodes SHOULD close all incoming and outgoing data connections to the applications
take over other networks.
Management nodes SHOULD run the management plane's pods and they SHOULD only run those
Take over the data connections of the management plane.
If used, storage nodes SHOULD only operate the pods of the storage services in the cluster.
APP.4.4 Kubernetes
Page 7 of 8
APP.4.4.A15 Separation of applications on node and cluster level (H)
Applications with a very high protection requirement SHOULD each have their own Kubernetes cluster or
use dedicated nodes that are not available for other applications.
APP.4.4.A16 Use of operators (H)
The automation of operational tasks in operators SHOULD be particularly critical
Applications and the programs of the management plan are used.
APP.4.4.A17 Attestation of nodes (H)
Nodes SHOULD have a secure one that is cryptographically and, if possible, verified with a TPM
Send status message to the management plane. The management plane SHOULD ONLY be nodes in
join the cluster who have successfully demonstrated their integrity.
APP.4.4.A18 Use of micro-segmentation (H)
Even within a Kubernetes namespace, the pods SHOULD only use the necessary network ports
can communicate with each other. There SHOULD be rules within the CRF, all but one
the network connections necessary for operation within the Kubernetes namespace
prevent. These rules SHOULD precisely define the source and destination of the connections and for them
Use at least one of the following criteria: service name, metadata ("labels"), the Kubernetes
Service accounts or certificate-based authentication.
All criteria that serve as a designation for this connection SHOULD be secured in such a way that they
can only be changed by authorized persons and administrative services.
APP.4.4.A19 High availability of pods (H)
The operation SHOULD be structured in such a way that in the event of a failure of one location, the applications in the pods
either continue to run without interruption or in a short time at another location
can start. All necessary configuration files, images,
User data, network connections and other resources required for operation, including the
The hardware required for operation must already be available at this location. For the
Uninterrupted operation of the cluster SHOULD Kubernetes based on the pods of the applications
distribute location data of the nodes over several fire compartments in such a way that the failure of one
Fire compartment does not lead to the failure of the application.
APP.4.4.A20 Encrypted data storage for pods (H)
The file systems with the persistent data of the management plane (especially etcd here) and the
Application services SHOULD be encrypted.
APP.4.4.A21 Regular restart of pods (H)
If there is an increased risk of external influences and a very high protection requirement, pods SHOULD be
regularly stopped and restarted. No pod SHOULD run for more than 24 hours. Included
The availability of the applications in the pod SHOULD be ensured.



4. Additional information
4.1. useful information
Further information on hazards and safety measures in the area of containers
can be found in the following publications:
• NIST 800-190
https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf
APP.4.4 Kubernetes
Page 8 of 8
• CIS benchmark Kubernetes
https://www.cisecurity.org/benchmark/kubernetes/
• OCI - Open Container Initiative
https://www.opencontainers.org/
• CNCF - Cloud Native Computing Foundation
https://www.cncf.io/


5. Appendix: Cross-reference table to elementary
Hazards
Safety measures can be derived from every requirement (A) in this module. the
Implementation of these measures counteracts those elementary threats (G0) that are relevant for the
Topic or target object are relevant. In the cross reference table (KRT) for this module are everyone
The corresponding elementary hazards are assigned to the requirement.
The KRT can be used to determine which elementary hazards are caused by which requirements
are covered. The letters in the second column indicate which basic values ​​the
Information security should be protected by the requirement as a priority. These core values ​​are
Confidentiality (C) for confidentiality, Integrity (I) for integrity and Availability (A) for
Availability.
The following elementary threats are important for the APP.4.4 Kubernetes module:
G 0.14 Spying on information / espionage
G 0.19 Disclosure of information worthy of protection
G 0.20 Information or products from an unreliable source
G 0.21 Manipulation of hardware or software
G 0.23 Unauthorized entry into IT systems
G 0.25 Failure of devices or systems
G 0.27 Lack of resources
G 0.28 Software weaknesses or errors
G 0.30 Unauthorized use or administration of devices and systems
G 0.37 Denial of actions
G 0.39 malicious programs
G 0.45 Loss of data
G 0.46 Loss of integrity of information worthy of protection











