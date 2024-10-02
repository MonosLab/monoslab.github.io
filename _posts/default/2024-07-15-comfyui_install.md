---
title: "[ ComfyUI ] ComfyUI 설치"
sub: post
author: Kwangsoo Seo
date: 2024-07-15 08:00:00 +0900
categories: [A.I., ComfyUI]
tags: [ComfyUI, AI, install, 인공지능, 설치]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post44.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post44)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# ComfyUI 설치   
ComfyUI 설치 방법은 아주 쉽습니다.   
Github 저장소(https://github.com/comfyanonymous/ComfyUI#windows)으로 이동 하셔서 Direct link to download 라고 된 링크에서 파일을 다운로드 받고, 원하는 폴더에 압축을 풀기만 하면 됩니다. 그리고 아래에서도 설명하겠지만, 이미 AUTOMATIC1111을 사용하고 있다면 설치 해둔 LoRA, controlNet 파일을 공유하여 사용할 수 있습니다.  

# 모델 설치   
모델들은 CIVITAI 나 huggingface 또는 github 등에서 구하셔서 ComfyUI_windows_portable\\ComfyUI\\models에 있는 적절한 폴더에 넣어주시기만 하면 됩니다. 예를 들어 checkpoints 모델을 다운 받으셨다면, ComfyUI_windows_portable\\ComfyUI\\models\\checkpoints 폴더에 넣어 주시기만 하면 됩니다.  아래는 제가 자주 사용하는 기본 모델들의 다운로드 목록입니다.   
## checkpoints   
* ComfyUI_windows_portable\\ComfyUI\\models\\checkpoints 폴더에 저장   
✅ SDXL1.0 base 모델        sdXL_v10VAEFix.safetensors (https://civitai.com/api/download/models/128078)   
✅ SDXL1.0 refiner 모델     sdXL_v10RefinerVAEFix.safetensors (https://civitai.com/api/download/models/128080)   
✅ 실사모델                     majicMIX realistic (https://civitai.com/models/43331/majicmix-realistic)   
✅ 그림모델                     anything (https://civitai.com/models/66/anything-v3)   

## vae   
* ComfyUI_windows_portable\\ComfyUI\\models\\vae 폴더에 저장   
✅ 실사VAE                     vae-ft-mse-840000-ema (https://huggingface.co/stabilityai/sd-vae-ft-mse-original/tree/main)   
✅ 그림VAE                     KI-f8-anime (https://civitai.com/models/118561/anything-kl-f8-anime2-vae-ft-mse-840000-ema-pruned-blessed-clearvae-fp16cleaned)   

# 기존 모델 사용하기   
ComfyUI_windows_portable\ComfyUI로 이동하면 extra_model_paths.yaml.example 파일이 있습니다. 편집기를 이용하여 오픈하시고 AUTOMATIC1111의 모델이 있는 곳의 위치를 설정하신 후 extra_model_paths.yaml로 새 이름으로 저장하시면 됩니다. 
base_path를 AUTOMATIC1111이 설치된 폴더로 지정하시면 됩니다. 
```   
a111:
    base_path: 여기에 전체 경로로 입력합니다. (ex : c:/sd/stable-diffusion-webui)

    checkpoints: models/Stable-diffusion
    configs: models/Stable-diffusion
    vae: models/VAE
    loras: |
         models/Lora
         models/LyCORIS
    upscale_models: |
                  models/ESRGAN
                  models/RealESRGAN
                  models/SwinIR
    embeddings: embeddings
    hypernetworks: models/hypernetworks
    controlnet: models/ControlNet
```   
