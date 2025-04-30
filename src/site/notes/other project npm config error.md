---
{"dg-publish":true,"permalink":"/other-project-npm-config-error/"}
---


다른 프로젝트에서 npm install 이 안되는 문제가 발생했었다.

```zsh

[기본 설정으로 되돌리는 코드]
joshmoon827@192 minari-web3.0 % npm config set registry https://registry.npmjs.org/

[확인코드]
joshmoon827@192 minari-web3.0 % npm config get registry                   
=> https://registry.npmjs.org/

```

npm config registry 를 private registry에서, 기본설정으로 다시 바꿔줘야했다.


메모:
next js 프로젝트에서 npm install -g next 를 해줘서 cli를 설치했던것 같다.