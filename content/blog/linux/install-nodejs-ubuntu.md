---
title: install nodejs ubuntu 20.04
date: 2021-07-05 16:07:54
category: linux
thumbnail: { thumbnailSrc }
draft: false
---

`node 14`를 `ubuntu 20.04`에 설치하는 방법
- apt 저장소에  `node 14` source 추가
- `gcc g++ make` 설치
- `nodejs yarn` 설치

```
# apt 저장소 저장
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

# dev tools 설치
sudo apt-get install gcc g++ make

# nodejs 설치
sudo apt-get install -y nodejs

# yarn 설치
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install -y yarn
```