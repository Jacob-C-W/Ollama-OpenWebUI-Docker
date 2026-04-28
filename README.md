# Ollama-OpenWebUI-Docker
Docker compose file for [Ollama](https://ollama.com/) and [OpenWebUI](https://github.com/open-webui/open-webui)

First we need to setup a spare PC or server with [Ubuntu Server](https://ubuntu.com/download/server), unless you already have a machine running docker or would prefer a virtual machine. 

## Setup machine with Ubuntu Server

    enter (english is selected by default)
    
    done (keyboard is english by default)
    
    Ubuntu Server (full)
    
    make sure Eth0 is pulling DHCP
    
    Enter (no proxy address)
    
    Make sure mirror works and then enter
    
    Use an entire disk
    
    done (don’t touch anything)
    
    user, domain-server-1 or domain-ollama-1, user, password, password.
    
    Done, skip ubuntu pro
    
    install openssh
    
    install stable docker then done
#

**Next let’s work on getting basic docker dependencies built.**
Click here to learn more about [Docker Compose](https://docs.docker.com/compose/).

    !
    sudo apt update
    sudo apt install -y \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    !
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
      sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    !
    echo \
      "deb [arch=$(dpkg --print-architecture) \
      signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    !
    echo \
      "deb [arch=$(dpkg --print-architecture) \
      signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    !
    sudo apt update
    sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    !
    docker --version
    docker compose version
    !
    #see that both docker and docker compose installed correctly
    !
    sudo systemctl enable docker
    sudo systemctl start docker
    !
    sudo usermod -aG docker user
    !
#

Now we can get started. 

## Setup Docker Compose Project Ollama
At this point you should SSH into the server from a PC so you can easily copy in the configuration. 

    mkdir Ollama

I prefer to keep my apps with multiple container dependencies seperate in their own folder for organization's sake. That way we can Docker-Compose Up Ollama 

I typically use Nano, but Vim works as well! Make sure you Nano or Vim in the new Ollama directory.
    
    sudo apt install nano

Copy the compose details on your PC then right-click in the terminal to paste the config. Then for Nano you need to "control + X" then "Enter" to exit and save the text editor.

    nano docker.compose.yml

Copy the details from docker-compose.yml file in the repository here. 

#

Now we can start up this compose and pull the images and finally start the containers. It may take a while to download the images depending on your internet, and then OpenWebUI can take a while to boot, so please be patient. 

        cd ollama/
        docker-compose up -d

## Open-WebUI Configuration
 
http://localhost:11434 will show ollama is running while http://localhost:3000 will host the webui. 

First, make sure Ollama is running at http://localhost:11434 

Then, after a while go to localhost:3000 to begin the setup process. Be aware it can take Open-WebUI time to startup. 

Enter name, email, password

Under connections @http://ollama:11434 goto manage and pull the model desired.

*Models can be found at: https://ollama.com/search*

**My advice**, find and tune models using trusted LLOMs like [Claude](https://claude.ai/). Let it know you're using open-webui, ollama models, and your machine specs and use that context to configure or find models for best results. Go ahead and provide it your use case as well to get the best results. Example: I need a model to help me learn code, I need a model to help me with my kid's homework, I need a model for gardening, etc. 


