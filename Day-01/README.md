## Day 1: Create Key Pair
### Task Objective
1.	Name of the key pair should be devops-kp.
2.	Key type should be RSA
3.	Resource should be created in us-east-1

### Steps
EC2 -> Network & Security -> Create Key Pair
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/63fa2d9d-0281-4962-b312-6c7facdb3ae0" />

## Additional notes:
## Generate RSA key-pair in your local and import it to AWS account:
### 1.	Practice "Bring Your Own Key" (Import Key Pair)
In production DevOps environments, companies usually never let AWS generate the private key. For security compliance, engineers generate the keys locally on a secure corporate laptop and only upload (import) the public key to AWS. This guarantees that AWS never touches your private credentials.  You can simulate this inside your lab's terminal :Generate a completely new pair locally in the lab's terminal:
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/db89e3e1-cfe7-4be4-b5a7-7d2db9617d41" />

This single command is the industry standard for creating a secure cryptographic key pair from a command-line terminal. Instead of asking AWS to generate the keys, this command handles the math right on your local machine.
Here is the exact breakdown of what every single piece (or flag) of that command actually tells your operating system to do:

**ssh-keygen -t rsa -b 4096 -f ~/devops-custom-key -N ""**


----------------------------------------------------------------------------------
| Component | What is means | Why it matters |
|--- | --- | --- |
| ssh-keygen | The Tool | This is a native Linux/Mac utility designed specifically to manufacture, manage, and convert authentication keys for SSH.|
| -t rsa | The Type | Specifies the cryptographic algorithm |
| -b 4096 | The Bits (Strength) | Controls the key length. By default, RSA creates 2048-bit keys. Forcing it to 4096 bits makes the key significantly longer and geometrically harder for a hacker to crack via brute-force. |
| -f ~/devops-custom-key | The File Path | Tells the system where to save the files and what to name them. The tilde (~) means your home directory. It outputs two distinct files: • devops-custom-key (The Private Key), • devops-custom-key.pub (The Public Key) |
| -N "" | The New Passphrase | Capital -N sets the password. Passing empty quotation marks ("") means "do not put a password on this key file." This is critical for automated DevOps CI/CD pipelines so scripts can use the key without a human being sitting there typing a password. |

**The Output:**
When you press enter on that command, your computer generates a matched pair of files that function like a lock and a physical key:

**•	devops-custom-key (Private Key)**: This stays on your local machine. Think of it like your physical house key. You must protect it and never share it.

**•	devops-custom-key.pub (Public Key)**: Think of this like a padlock. You can hand copies of this padlock out to anyone. 

------------------------------------------------------------------------------------------------------------------

### Step 2: Import that public key to AWS using the CLI: ###
<img width="940" height="222" alt="image" src="https://github.com/user-attachments/assets/2366f504-bc2c-49fd-90ce-4072f055cd23" />

This command is an API call that takes the public key padlock you just generated on your local Linux terminal and uploads it to AWS's cloud database so AWS can use it to secure your future EC2 instances.
Here is the exact breakdown of what each parameter and flag is doing behind the scenes:

**What happens in AWS when you run this?**

When this command executes successfully:
1.	The AWS CLI reads the contents of your devops-custom-key.pub file.
2.	It sends an HTTPS request to the AWS EC2 endpoint.
3.	AWS registers this public key under the name devops-imported-kp inside the specific region you are working in.
4.	It outputs a JSON block in your terminal confirming the key's name and its unique cryptographic fingerprint.
Now, if you go to launch an EC2 instance via the AWS UI Console, devops-imported-kp will instantly appear as an option in your dropdown menu!
<img width="700" height="150" alt="image" src="https://github.com/user-attachments/assets/8a65e80e-5472-4d79-864a-519e90eb3ea3" />



