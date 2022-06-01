# Node版本切换

## nrm
``` bash
npm install -g nrm

nrm ls   //查看镜像源, *表示正在使用的

  npm ---------- https://registry.npmjs.org/
  yarn --------- https://registry.yarnpkg.com/
  tencent ------ https://mirrors.cloud.tencent.com/npm/
  cnpm --------- https://r.cnpmjs.org/
  taobao ------- https://registry.npmmirror.com/
  npmMirror ---- https://skimdb.npmjs.com/registry/   

nrm use cnpm //切换

npm get registry

```

## n
``` bash
sudo npm i -g n

n -V

n ls

sudo n 14.19.3

n

n rm 14.19.3

node -v

```