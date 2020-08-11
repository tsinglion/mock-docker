# UOS docker 中通过 mock 编译 rpm

## UOS 欧拉版说明

1. 下载docker 镜像并导入镜像

    ```bash
    wget http://10.7.10.100/euler/fuyu/release/publish/uos-docker-20-arm64.tar.xz
    docker docker load -i uos-docker-20-arm64.tar.xz
    ```

2. 创建编译工作目录，存放当前需要编译的 rpm 包的 spec 和源码包

    ```bash
    BUILD121=/tmp/build_121
    mkdir $BUILD121/SOURCES/  ##  自行创建目录
    # cp /path/*.spec /patch/*.tar.gz $BUILD121/SOURCES/ 　## 拷贝spec 和 源码到  SOURCES 目录下
    ```

3. 更新当前仓库

    ```bash
    git pull
    ```

4. 在 仓库的 `uoseuler` 目录下通过 `Dockerfile` 构建新的容器 `uoseuler/mock-rpmbuilder`

    ```bash
    cd <path_to_git_repo>/uoseuler
    docker build -t uoseuler/mock-rpmbuilder ./
    ```

5.  开始构建 rpm , 其中 `MOCK_CONFIG` 变量值为固定， `SOURCES` 变量值为相对目录， 相对与 -v 参数后跟的挂载目录 `/tmp/build_121/` 

    ```bash
    docker run --cap-add=SYS_ADMIN -v $BUILD121/:/rpmbuild \
     -e MOCK_CONFIG=uel20-aarch64 -e SOURCES=SOURCES -e SPEC_FILE=SOURCES/dtkwidget.spec  \
    uoseuler/mock-rpmbuilder
    ```

6. 编译生成的源码包和二进制包在 `$BUILD121/output/uel20-aarch64` 目录下。