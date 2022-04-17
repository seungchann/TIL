# Computer Security  
## Software Lifecycle  

### 1. Software liveliness  
* Software is not a one-shot effort but **evolves**  
* Software development, production, and maintenance are cost/labor intensive.  
* Software life-time can **outlive** hardware.  

### 2. Software Engineering  
* A process of analyzing user requirements and then designing, building, and testing software application which will satisfy those requirements.  

### 3. Secure Software Engineering  
* Incorporate security throughout the software development lifecycle.  
  * Prevent loss / corruption access to data  
  * Prevent unauthorized access to data  
  * Prevent unauthorized computation  
  * Prevent escalation of privileges  
  * Prevent downtime of resources  
* Secure SE focuses on  
  * limiting functionality  
  * enforcing security policies  
  * defining constraints  

### 4. Secure Development Cycle  
* A secure SDLC involves **integrating security testing and other activities** into an existing development process  
* SDLC: Software/System Development Life Cycle  

#### 1) SDLC: Requirement analysis  
* Define **scope of project and secuirty boundaries.**  
* Define security / privacy specification, identify assets, assess environment, and specify use / abuse cases.  
  * Threat modeling  
    * threats  
    * attack vectors  
    * emergency plans  
  * Security requirements  
    * privacy policy  
    * data management plan  
  * Third party dependencies  
    * define third party dependencies along with their update policies  
    * risk analysis on dependencies  

#### 2) SDLC: Design  
* The classic design phase focuses on functionality requirements.  
* We make **security concerns** an integral part of the analysis.  
  * Continuously update threat model as requirements change  
  * Security design review  

#### 3) SDLC: Implementation  
* The design may be slightly refined  
* The security documents must be updated accordingly along with continuous reviews and analysis.  
  * **Continuous code reviews** ensure high code quality and highlights flaws  
  * **Static analysis** ensures high code quality and highlights flaws.  
  * **Vulnerability scanning** of external dependencies for exploits.  
  * **Unit tests** ensure functionality / security across components.  
  * **Accountability**
    * use a source code / version control system  
  * **Coding standards**  
    * assertions and documentation  
  * **Continuous integration**  
    * run unit tests, static analysis, and linter whenever code is checked in  

#### 4) SDLC: Testing  
* Completed components are tigorously tested before they are finally integrated into the prototype.  
  * **Fuzzing** is a form of probabilistic test generation.  
  * **Dynamic analysis** complements fuzzing with heavy-weight tests based on symbolic execution and models.  
  * Third party penetration testing provides external validation.  

#### 5) SDLC: Release  
* Before release of the final prototype, verify the base assumptions from the initial requirement analysis and design.  
  * **Security review**
    * check for compliance of security properties  
  * **Privacy review**  
    * check for privacy policy compliance  
  * Review all licensing agreements (e.g., for open source software)  

#### 6) SDLC: Maintenance  
* After shipping software, continuously maintain **security properties.**  
  * Track third-party software and update accordingly  
  * Provide vulnerability disclosure contacts through
    * e.g., a bug bounty program or at least a public contact  
  * Regression testing  
    * whenever an update is deployed recheck security and functionality requirements.  
  * Deploy updates securely  

### 5. Summary  
* Secure software development enforces security principles during software development  
* Software lives and evolves  
* Security mustt be first class citizen  
  * Secure Requirements / specification  
  * Security-aware Design (Threats?)  
  * Secure Implementation (Reviews?)  
  * Testing (Red team, fuzzing, unit)  
  * Updates and patching  
