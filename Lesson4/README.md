### Getting started
* ```git clone``` this repository
* ```nix-build -E "with import <nixpkgs> {}; callPackage <your-dir>/k8slens.nix {}"```

###### launch app: 
```./result/bin/k8slens```

#### or package it by yourself:
* Download AppImage for Kubernetes Lens [https://k8slens.dev/]
* Wrap it and pack it like k8slens.nix
* generate sha: ```nix-prefetch-url <Download-URL>```
* ```nix-build k8slens.nix```
* launch app
