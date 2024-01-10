# Golang配置

## go 版本管理
```bash
gvm install go1.12.17 -B
```

## Go常用配置
版本配置文件(${HOME}/.gvm/environments/default)
```bash
export GVM_ROOT; GVM_ROOT="${HOME}/.gvm"
export gvm_go_name; gvm_go_name="go1.18.7"
export gvm_pkgset_name; gvm_pkgset_name="global"
export GOROOT; GOROOT="$GVM_ROOT/gos/go1.18.7"
export GOPATH; GOPATH="/mnt/work/go"
export GOPROXY; GOPROXY="https://goproxy.cn"
export GOCACHE; GOCACHE="${GOPATH}/.cache/go-build"
export GOLANGCI_LINT_CACHE; GOLANGCI_LINT_CACHE="${GOPATH}/.cache/golangci-lint"
export STATICCHECK_CACHE; STATICCHECK_CACHE="${GOPATH}/.cache/staticcheck"
export GOPRIVATE; GOPRIVATE="gitlab.xxx.com"
export GVM_OVERLAY_PREFIX; GVM_OVERLAY_PREFIX="${GVM_ROOT}/pkgsets/go1.18.7/global/overlay"
export PATH; PATH="${GVM_ROOT}/pkgsets/go1.18.7/global/bin:${GVM_ROOT}/gos/go1.18.7/bin:${GVM_OVERLAY_PREFIX}/bin:${GVM_ROOT}/bin:${GOPATH}/bin:${PATH}"
export LD_LIBRARY_PATH; LD_LIBRARY_PATH="${GVM_OVERLAY_PREFIX}/lib:${LD_LIBRARY_PATH}"
export DYLD_LIBRARY_PATH; DYLD_LIBRARY_PATH="${GVM_OVERLAY_PREFIX}/lib:${DYLD_LIBRARY_PATH}"
export PKG_CONFIG_PATH; PKG_CONFIG_PATH="${GVM_OVERLAY_PREFIX}/lib/pkgconfig:${PKG_CONFIG_PATH}"
```

## 网络配置
网络配置文件(${HOME}/.netrc)
```bash
machine xxx.com
login demo
password demo
```

## govendor使用(废弃)
```bash
go mod init
go mod tidy
go mod vendor
govendor fetch k8s.io/code-generator/^@c2090be
govendor fetch k8s.io/code-generator/^@kubernetes-1.13.4
govendor update k8s.io/code-generator/...
```