# Description
This is a local model tester. You can download and try out the models from [ollama website](https://ollama.com/library)

Example models:
- mixtral model
- mistral 
- llava for image processing => image dude, does not recognize its own picture
- dolphin-mixtral (fine tuned mixtral for coding) => big dude
- dolphin-mistral (fine tuned mistral for coding) => small dude

# Prerequisites
It is recommended to use a GPU in each case. It will return you much quicker results.

For NVIDIA card perform the following steps based on debian in order to :

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt update
sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctrl restart docker
sudo apt install -y cuda-toolkit
sudo apt install -y nvidia-gds

# if it does not work with the first run
reboot

```


run server with docker:
```bash
# if gpus are not recognized use
docker run -d --gpus=all -v ollama:/root/.ollama -v $(pwd)/files:/source --name=ollama  -p 11434:11434 ollama/ollama
# starting a chat with model llava
docker exec -it ollama ollama run llava
# mixtral => 26GB , mistral 3GB, llama2 (meta), dolphin-mixtral (finetuning mixtral for coding)

# remove a model
docker exec -it ollama ollama rm llava
# list models
docker exec -it ollama ollama list
```

Or with docker compose:
```bash
# make sure gpu is detected if not reboot your system
docker compose up -d && docker compose logs -f
```



This will start a prompt and reference the image:
`extract the information from the following image. It is an invoice position return me the information as json of this image! /source/position.png`


```javascript
node
fs.writeFileSync('./files/position.base64', fs.readFileSync('./files/position.png', {encoding: 'base64'}));
```