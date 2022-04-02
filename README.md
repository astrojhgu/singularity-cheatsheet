# singularity-cheatsheet

## 基本概念：`沙箱`和`镜像`
`沙箱`和`镜像`是两种可供singularity调用的数据，并且可以相互转换。
- `沙箱`的表现形式是一个目录
- `镜像`的表现形式是一个文件，通常以`.simg`或者`.sif`作为扩展名。

## 符号声明
以下
- 以`${SANDBOX}`代表沙箱的名称
- 以`${SIMG}`代表镜像的名称
- 不加区分时，则以${ENV}代表两者之一。

## 进入singularity shell 交互环境
```bash
singularity shell ${ENV}
```

## 不进入交互环境，而只执行某条命令
```bash
singularity exec ${ENV} ${CMD} [${ARG1} [${ARG2} [...]]]
```

## 如果发现某一个磁盘卷在交互环境和命令执行中无法访问，那是因为singularity默认只挂在最少的必要磁盘卷，应加上下列参数：
`-B ${PATH_IN_HOST}:${MOUNT_POINT_IN_SINGULARITY}`
例如，位于`/data1`上的数据无法被看到，应该执行下述命令
```bash
singularity shell -B /data1:/data ${ENV}
```
进入交互环境中之后，本机上`/data1`磁盘卷将被挂载到`/data`。

## 定义文件、沙箱、镜像之间的转换
- 可以从`沙箱`构建`镜像`
- 可以从`镜像`构建`沙箱`
- 可以从`定义文件`构建`沙箱`或者`镜像`
方法：
### 从定义文件到镜像
```bash
sudo singularity build ${SIMG} ${DEF.def}
```
### 从定义文件到沙箱
```bash
sudo singularity build --sandbox ${SANDBOX} ${DEF.def}
```
### 从沙箱到镜像
```bash
sudo singularity build ${SIMG} ${SANDBOX}
```

### 从镜像到沙箱
```bash
sudo singularity build --sandbox ${SANDBOX} ${SIMG}
```
