# Computer Security  
## Software security basic principle  

### 1. What is computer security?  
* A process and the collection of measures and controls that ensures **the Confidentiality, Integrity and Availability (CIA)** of the assets in computer systems.  

### 2. CIA security triad  
* Confidentality  
  * An attacker cannot recover protected data  
* Integrity  
  * An attacker cannot modify protected data  
* Availability  
  * An attacker cannot stop/hinder computation  

![CIA Triad](https://user-images.githubusercontent.com/63276842/162602987-c0a28e07-526a-454a-af3e-6ad6c74b855c.png)  

### 3. Authentication & Non-repudiation  
* Authentication  
  * The ability of a computer system to confirm the sender’s identity.  
* Non-repudiation or accountability  
  * The ability of a computer system to confirm that the sender can not deny about something sent.  

### 4. Software security  
* Foci of the software security  
  * testing
  * evaluating 
  * improving  
  * enforcing  
  * proving the securty of software  
* Allow **intended** use of software
* Prevent **unintended** use that may cause harm  

### 5. Threat model  
* It defines **the abilities and resources** of the attacker.  
* Examples  
  * A class of attacks that you want to stop  
  * What is the attacker's capability?  
  * What is impact of an attack?  
  * What attacks are out-of-scope?  

### 6. Fundamental security mechanisms  
* Isolation  
* Least privilege  
* Fault compartments  

### 7. Isolation  
* Isolate two components from each other.  
* One component **cannot access** data/code of the other component except through a well-defined API.  
* Example: User-space application  
  * It may only access the disk through the filesystem API.  
  * The OS prohibits direct block access to raw data.  
  * The OS isolates the user-space process from the disk.  

### 8. Least privilege  
* It ensures that a component has the least privileges needed to function.  
* Any further removed privilege reduces functionality  
* Any added privilege will not increase functionality  
* This property constraints an attacker in the obtainable privileges  

### 9. Fault compartments  
* It builds on **least privilege and isolation.**  
* Both properties are most effective in combination.  
  * Many small components that are running and interacting with least privileges.

### 10. Hardware and software abstractions  
* **Abstraction** is the act of representing essential features without including the background details or explanations.  
* **Abstractions** allow an encapsulation of ideas without having to go into implementation details.  
* Examples
  * Software: An API abstracts the underlying implementation by defining how a library can be used  
  * Operating systems: The system call interface abstracts low level implementations  
  * Hardware: An ISA (Instruction set architecture) abstracts the underlying implementation of the instructions into logic and state  

### 11. Abstractions  
#### 1) Single domain OS  
* A single layer, no isolation or compartmentalization
* All code runs in the same domain: 
  * The application can directly call into operating system drivers
* High performance, often used in embedded systems  

#### 2) Monolithic OS  
* Two layers: the operating system and applications  
* The OS **manages resources and orchestrates access**  
* Applications are **unprivileged**, must request access from the OS  
* Linux(fully) Windows(mostly) follows this approach for performance  
  * isolating individual components is expensive  

#### 3) Micro-kernel  
* Many layers: each component is a separate process  
* Only essential parts are privileged  
  * Process abstraction (address spaces)  
  * Process management (scheduling)  
  * Process communication (IPC)  

### 12. Access control  
* **Access Control** is the process of enforcing the required security for a particular resource.  
* Prevent that user from accessing anything that they should not be able to.  
* Access Control can be seen as the combination of **Authentication and Authorisation** plus additional measures, such as clock- or IP-based restrictions.  

### 13. Authentication  
* Three fundamental types of identification  
  * What you know (ex. username / password)  
  * What you are (ex. biometrics)  
  * What you have (ex. smartcard / phone)  
* Authentication is stronger in combination  
  * Two factors are better than one  
  * Check for username/password and presence of a security token  

### 14. Authorization: Information Flow Control (IFC)  
> Who can access what information?  
* Access policies are called **access control models.**  
* Three types of access control  
  * Mandatory Access Control (MAC)  
  * Discretionary Access Control (DAC)  
  * Role-Based Access Control (RBAC)  

#### 1) MAC  
> Idea: rule and lattice-based policy  
* Centrally controlled  
* **One entity** controls what permissions are given  
* Users cannot change policy themselves  

#### 2) DAC  
> Idea: object owner specifies policy  
* User has authority over her resources (introduce ownership)  
* User sets permissions for her data if other users want access  
* Example: Unix permissions  
* Access control matrix  
  * A table of subjects and objects indicating what actions individual subjects can take upon individual objects  

#### 3) RBAC  
* Policy defined in terms of roles (sets of permissions).  
* Individuals are assigned roles.  
* Roles are authorized for tasks.  
  * Access permission is broken into sets of roles.  
  * Users get assigned specific roles.  
  Administration privileges may be a role.  

### 15. Summary  
* Software security goal  
  * Allow **intended** use of software  
  * Prevent **unintended** use that may cause harm  
* Security triad, core principles  
  * Confidentiality  
  * Integrity  
  * Availability  
* Security of a system depends on its threat model.  
* Concepts:
  * Isolation
  * Least privilege  
  * Fault compartments  
* Abstraction and access control further enforce security.  
