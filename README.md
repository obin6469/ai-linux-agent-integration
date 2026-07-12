# ai-linux-agent-integration
#Deployment and configuration of AI CLI agents on Linux servers using ChatGPT CLI, Shell Genie, Aider, and Goose CLI.

# AI CLI Agents Deployment on Linux Servers

# Overview

#This project documents the deployment and configuration of AI-powered CLI agents on Linux servers to support software development, system administration, automation, and technical documentation.
#The goal is to create a secure and maintainable environment where AI assistants can help engineers improve productivity while following Linux security and operational best practices.

# Architecture

#The deployment follows an isolation-first approach:

<img width="201" height="250" alt="Archtecture 2" src="https://github.com/user-attachments/assets/06c89482-43f9-4d64-994e-25765b517709" />



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
# Chatgpt CLI
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
# install shell Genie
pipx install shell-genie
# verify 
pipx list

shell-genie --version
# Initial error:
#TypeError: Secondary flag is not valid for non-boolean flag.

#run this command to fix typer/click compatibility issue

pipx runpip shell-genie install "click<8.2"
# initialise shell-genie
shell-genie init
# Select backend from 2 prompt 
[openai-gpt-3.5-turbo/free-genie]:

#enter openai api key

# start shell-genie
shell-genie ask "analyse fstab file"

<img width="564" height="214" alt="shell-genie 1" src="https://github.com/user-attachments/assets/d8d11230-6564-4d37-8ccc-9a2315d127d3" />

#################
# Installing Aider
######

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

#pipx is used to install Aider in an isolated virtual environment. This prevents dependency conflicts with other Python applications running on the server.

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

#Goose is an AI agent designed for more autonomous workflows.

# Create a separate Linux account
useradd -m -s /bin/bash goose

#add the user to sudoers file

usermod -aG wheel goose

#set the user password

passwd goose
# create the project directory
mkdir -p /home/goose/projects

chown -R goose:goose /home/goose/projects

#switch to goose user

su - goose

# installation of goose
curl -fsSL https://github.com/block/goose/releases/latest/download/install.sh | bash

#verify 

goose --version

# Configure goose

#create goose config

mkdir -p ~/.config/goose

#edit configuration

vi ~/.config/goose/config.yaml

#example of config

provider: openai

model: gpt-4.1

workspace:

  directory: /home/goose/projects

security:

  isolated: true

# Configure OpenAI API Access

# Configure the API key for the dedicated Aider user:
export OPENAI_API_KEY="your_api_key"

# To make the configuration persistent:
echo 'export OPENAI_API_KEY="your_api_key"' >> ~/.bashrc
source ~/.bashrc

# Create automation workplace

mkdir -p ~/projects/my-agent-task

cd ~/projects/my-agent-task

#initialise git

git init

# Run goose CLI
goose

<img width="949" height="348" alt="goose 1" src="https://github.com/user-attachments/assets/64ca5690-4fae-48da-ad06-cd6dd92dd0af" />




# Conclusion

#This project demonstrates a practical approach to deploying AI CLI agents on Linux servers using secure operating practices.

#By combining:

#User isolation

#Environment separation

#Git workflows

#AI-assisted development and operations

#=>AI tools can become reliable assistants for modern engineering and system administration workflows.    

########## 
