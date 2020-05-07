# Instalando o OpenSceneGraph (OSG)

http://www.openscenegraph.org/

Salvar e descompactar o repositório (eu coloquei em ~/Downloads).

https://github.com/openscenegraph/OpenSceneGraph/tree/OpenSceneGraph-3.6.5

## Debian 10

Para ver a versão da placa de vídeo e outras informações. Os dados abaixo são do meu PC.
```
# apt install mesa-utils
$ glxinfo

OpenGL vendor string: NVIDIA Corporation
OpenGL renderer string: GeForce GTX 750 Ti/PCIe/SSE2
OpenGL core profile version string: 4.6.0 NVIDIA 418.74
OpenGL core profile shading language version string: 4.60 NVIDIA

$ glxinfo  | grep rendering
direct rendering: Yes (usa aceleração 3D)

$ glxgears -info
300 frames in 5.0 seconds = 59.883 FPS
```

O que eu precisei instalar.
```
# apt install cmake libgl1-mesa-dev
(You may also want libglu1-mesa-dev, freeglut3-dev)

$ cd ~/Downloads/OpenSceneGraph-OpenSceneGraph-3.6.5
$ cmake .
$ make
# make install

A documentação fala de # make install_ld_conf, mas comigo não funcionou. Ao invés disso, precisei

# vim /etc/ld.so.conf.d/openscenegraph.conf
Inserir a linha
${CMAKE_INSTALL_PREFIX}lib${LIB_POSTFIX}
# ldconfig
```

Baixar os dados de exemplo.

```
http://www.openscenegraph.org/index.php/download-section/31-data
Baixei do link OpenSceneGraph-Data-3.4.0.zip
(http://www.openscenegraph.org/downloads/stable_releases/OpenSceneGraph-3.4.0/data/OpenSceneGraph-Data-3.4.0.zip)

$ cd ~/Downloads/OpenSceneGraph-Data/
$ osgviewer cow.osg
OK - já dá pra ver a vaca metálica flutuando, podendo ser manipulada com o mouse.
```
