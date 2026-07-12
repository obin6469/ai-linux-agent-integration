# ai-linux-agent-integration
#Deployment and configuration of AI CLI agents on Linux servers using ChatGPT CLI, Shell Genie, Aider, and Goose CLI.
#AI CLI Agents Deployment on Linux Servers

# Overview

#This project documents the deployment and configuration of AI-powered CLI agents on Linux servers to support software development, system administration, automation, and technical documentation.
#The goal is to create a secure and maintainable environment where AI assistants can help engineers improve productivity while following Linux security and operational best practices.

# Architecture

#The deployment follows an isolation-first approach:

<img width="238" height="224" alt="Archtecture 1" src="https://github.com/user-attachments/assets/efd30581-21a3-4b66-96ca-03333601ab5d" />


# Each AI agent runs with:

#Dedicated Linux user account

#Independent configuration

#Separate credentials

#Controlled project access

#Installed AI CLI Agents

#ChatGPT CLI

# Used for
#Technical assistance
#Documentation generation
#Troubleshooting
#Research and explanations

Example:

How can I create and mount a new partition from a newly added disk on Linux?
<img width="948" height="304" alt="chatgpt CLI" src="https://github.com/user-attachments/assets/ee912a8d-9c5d-414a-bd8f-68a09c50579e" />

#################
 # OpenAI API Setup
#################
# Enable OpenAI API Billing
#The OpenAI API requires separate billing from ChatGPT subscriptions.
#Steps:
#Open the OpenAI Platform billing page.
#Add a payment method.
#Create an API key.
#Copy the API key immediately.
#Example API key format:sk-proj-xxxxxxxxxxxxxxxx

################
Chatgpt CLI
################
# install pip and python
dnf install -y python3-pip
# verify 
python3 -m pip --version
# install openai python sdk
python3 -m pip install --user openai
# verify
python3 -c "import openai; print(openai.__version__)"
# configure openai api key
echo OPENAI_API_KEY="sk-proj-your_key_here"
# Make API key permanent
echo 'export OPENAI_API_KEY="your_api_key"' >> ~/.bashrc
# Reload the shell
source ~/.bashrc
# verify
echo $OPENAI_API_KEY
# create chatgpt CLI application
vi ~/chatgpt.py
# Example of a chatgpt script
#!/usr/bin/env python3

from openai import OpenAI

client = OpenAI()

print("ChatGPT CLI")
print("Type 'exit' or 'quit' to close")

while True:
    question = input("\nYou: ")

    if question.lower() in ["exit", "quit"]:
        break

    response = client.responses.create(
        model="gpt-5-mini",
        input=question
    )

    print("\nChatGPT:")
    print(response.output_text)
# convert script into linux command
mkdir ~/.local/bin
# install chatgpt CLI command
mv ~/chatgpt.py ~/.local/bin/chatgpt
chmod +x ~/.local/bin/chatgpt
# start chatgpt
chatgpt
<img width="942" height="249" alt="chatgpt CLI 1" src="https://github.com/user-attachments/assets/92c6635c-62b7-4fa2-a7d6-c4712334edea" />

############
# Shell Genie
##############

#Shell Genie is a terminal assistant that helps generate and explain Linux commands.
# install pipx
#check if is already installed
pipx --version
#installation 
dnf install -y python3-pipx
# enable pipx path
python3 -m pipx ensurepath
# reload shell
source ~/.bashrc
# verify 
pipx --version


# Installing Aider on AlmaLinux 10.1
#EnvironmentOperating System: AlmaLinux 10.1
#Shell: Bash
#Python: Python 3.x
#Package Manager: pipx
#Version Control: Git
#AI Provider: OpenAI API

# Create a Dedicated User for Aider

#To follow Linux administration best practices, Aider is installed under a dedicated user account. This isolates the AI development environment from other system users and Python applications.

# Create the aider user
sudo useradd -m -s /bin/bash aider
# add user aider to sudoers file
sudo usermod -aG wheel aider
# Set a password if interactive login is required:
sudo passwd aider

# Switch to the Aider user:
su - aider

# Verify the user environment:
id aider

# Install required packages:
sudo dnf install python3-pip pipx git -y

# Configure the local Python application path:
pipx ensurepath

# Reload the shell:
source ~/.bashrc

# Install Aider Using pipx
# pipx is used to install Aider in an isolated virtual environment. This prevents dependency conflicts with other Python applications running on the server.

# Install Aider:
pipx install aider-chat

# Verify installation:
aider --version

# Configure OpenAI API Access

# Configure the API key for the dedicated Aider user:
export OPENAI_API_KEY="your_api_key"

# To make the configuration persistent:
echo 'export OPENAI_API_KEY="your_api_key"' >> ~/.bashrc
source ~/.bashrc

# Verify the variable:

echo $OPENAI_API_KEY
# start aider 
# create a directory dedicated to your aider projects
mkdir -p ~/projects
# initiate Git
git init
# create a python file
cat > app.py <<EOF
def main():
    print("Hello world")

if __name__ == "__main__":
    main()
EOF
# start aider
aider app.py
## Images from AlmaLinux 10.1
<img width="483" height="329" alt="aider 2" src="https://github.com/user-attachments/assets/06589dc9-28ca-48ef-9ff5-6f6a7e9cdccf" />
<img width="938" height="435" alt="aider 1" src="https://github.com/user-attachments/assets/6c717a00-2b3c-4e29-9558-5087f72bf65a" />

# aider Capabilities:
#Code generation
#Refactoring
#Debugging
#Documentation
#Multi-file changes
#Git-aware workflows

###############
# Goose CLI
###############
Goose is an AI agent designed for more autonomous workflows.

Capabilities:

Project analysis
Multi-step tasks
Automation assistance
Tool-based workflows
Infrastructure support

Installation follows a dedicated user model:

/home/goose

with independent configuration and credentials.

Security Design

AI agents can read and modify files, therefore isolation is essential.

Implemented practices:

✅ Dedicated Linux users
✅ No execution as root
✅ Separate API credentials
✅ Git-based change tracking
✅ Controlled project directories

Recommended:

/home/aider/projects
/home/goose/projects

Avoid providing access to:

Private keys
Password files
Cloud credentials
Sensitive configuration files
Git Workflow

Git is used as a safety mechanism:

Create checkpoint
        ↓
Request AI changes
        ↓
Review diff
        ↓
Commit changes
        ↓
Deploy

Useful commands:

git diff
git status
git commit
Supported Use Cases

These AI agents can assist with:

Development
Python
Java
Go
Rust
C/C++
JavaScript / TypeScript
Infrastructure
Linux administration
Bash scripting
Ansible
Terraform
Docker
Kubernetes
Documentation
README files
Technical guides
Operational runbooks
Troubleshooting procedures
Strengths
Productivity

Reduces time spent on:

Writing repetitive code
Searching documentation
Creating scripts
Explaining complex systems
Knowledge Sharing

Helps engineers:

Understand unfamiliar code
Learn new technologies
Document environments
Automation

Useful for:

Infrastructure tasks
Maintenance scripts
Deployment workflows
Limitations

AI assistants require human validation.

Potential risks:

Incorrect code generation
Unsafe configuration changes
Wrong assumptions
Increased API usage costs

Best practices:

Review all generated changes
Use Git checkpoints
Provide clear project context
Avoid exposing secrets
Future Enhancements

Planned improvements:

Local LLM integration using Ollama
MCP-based tool integration
AI-assisted monitoring
Automated infrastructure reviews
CI/CD workflow integration
Conclusion

This project demonstrates a practical approach to deploying AI CLI agents on Linux servers using secure operating practices.

By combining:

User isolation
Environment separation
Git workflows
AI-assisted development and operations

AI tools can become reliable assistants for modern engineering and system administration workflows.    
