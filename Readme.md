
run server with:
```sh
# since gpus are not recognized
docker run -d --gpus=all -v ollama:/root/.ollama -v $(pwd)/files:/source --name=ollama  -p 11434:11434 ollama/ollama
# starting a chat with model llava
docker exec -it ollama ollama run llava
# mixtral => 26GB , mistral 3GB, llama2 (meta), dolphin-mixtral (finetuning mixtral for coding)
```

https://ollama.com/library/llava

This will start a prompt and reference the image:
`extract the information from the following image. It is an invoice position return me the information as json of this image! /source/position.png`


```javascript
node
fs.writeFileSync('./files/position.base64', fs.readFileSync('./files/position.png', {encoding: 'base64'}));
```