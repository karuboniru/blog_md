---
title: Geant4 基础
date: 2020-04-05 11:23:37
tags: 
- Geant 4
categories: 物理
---

## 安装

Geant4 的安装非常容易, 以我编译的配置为例: 

```bash
cmake    -DGEANT4_BUILD_MULTITHREADED=ON \
         -DGEANT4_INSTALL_DATA=ON \
         -DGEANT4_USE_GDML=ON \
         -DGEANT4_USE_G3TOG4=ON \
         -DGEANT4_USE_QT=ON \
         -DOpenGL_GL_PREFERENCE=GLVND \
         -DGEANT4_USE_XM=ON \
         -DGEANT4_USE_OPENGL_X11=ON \
         -DGEANT4_USE_INVENTOR=OFF \
         -DGEANT4_USE_RAYTRACER_X11=ON \
         -DGEANT4_USE_SYSTEM_CLHEP=OFF \
         -DGEANT4_USE_SYSTEM_EXPAT=ON \
         -DGEANT4_USE_SYSTEM_ZLIB=ON \
         ..
make -jN
make install
```

就可以了, 想要更快的安装, 如果你用的是 Fedora 或者 Arch的话就有更方便的安装方式: 通过 [Copr](https://copr.fedorainfracloud.org/coprs/yanqiyu/geant4/) 或者是 [Aur](https://aur.archlinux.org/packages/geant4/) 进行安装. 步骤会变得非常简单. 

以 Copr 上的版本为例(因为那是我在前人基础上改的版本):

```bash
sudo dnf copr enable yanqiyu/geant4
sudo dnf install geant4 geant4-devel geant4-data geant4-examples
```

就可以完成安装. 

## 运行前准备

运行前需要设置使用的数据的环境变量, 如果使用的是 Copr 或者是 Aur 这类的打包好的版本的话相关环境变量会被设置好(在重新打开终端之后). 自己编译的话需要运行安装目录下的 `geant4.sh` 这个脚本

```bash
source /path/to/geant4.sh
```

## Geant4 框架

作为一个简单的 Geant4 程序, 需要包含的类有

- `physicsList`, 一般是 `G4VModularPhysicsList` 以及其派生的预定义物理类型, 这个类负责定义模拟中的物理过程;
- `DetectorConstruction`, 派生自`G4VUserDetectorConstruction` 负责构建探测器的几何结构以及材料;
- `ActionInitialization` 派生自 `G4VUserActionInitialization`, 主要工作是对于模拟进行准备操作, 通过 `SetUserAction` 来实现注册各个其他运行相关的类, 在简单的程序中包括 `RunAction`, `EventAction`, `SteppingAction`, `PrimaryGeneratorAction` , 如果需要保存直方图的话就要加上 `HistoManager` ;
- `RunAction` 派生自 `G4UserRunAction`, 这个类定义了每个 run 的操作;
- `PrimaryGeneratorAction` , 派生自 `G4VUserPrimaryGeneratorAction` , 也就送描述粒子源的行为;
- `EventAction` 派生自 `G4UserEventAction`, 描述的是一个模拟的 Event 的过程, 一般处理的事情是按照设计统计每个 Step 中物理过程的能量沉积等数据, 并填充 tuple;
- `SteppingAction` 派生自 `G4UserSteppingAction`, 处理模拟过程每个 step, 每个 step 中 `UserSteppingAction` 都会被调用, 并传入一个 `G4Step` 参数, 可以获取这个 step 中的物理过程.
- `HistoManager`, 它一般是负责保存直方图到 `ROOT` 文件以供进一步操作的;
- 对于一些类来说, 还可以有它的 `messenger` 顾名思义, 就是用来取得更详细的信息之用.
