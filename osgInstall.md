# Instalando e usando o OpenSceneGraph (OSG)

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
(Pode também ser preciso libglu1-mesa-dev, freeglut3-dev)

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
Já dá pra ver a vaca metálica flutuando, podendo ser manipulada:
 - botão esquerdo e arrastar: girar o modelo
 - botão direito e arrastar (ou rodinha do mouse): zoom
 - botão do meio (rodinha) e arrastar: mover o modelo
 - barra de espaço no teclado: voltar à posição inicial
```

Compilando osgTerrain

```
$ cd ~/Downloads/OpenSceneGraph-OpenSceneGraph-3.6.5/examples/osgterrain
$ g++ osgterrain.cpp -lOpenThreads -losg -losgDB -losgViewer -losgGA -losgTerrain -losgFX -o osgterrain
$ ./osgterrain 
Warning: no valid data loaded, please specify a database on the command line.
```

Agora é achar que tipo de dados ele pede.

Ainda não encontrei documentação pra classe osgTerrain, que já vem com o osg. Mas encontrei outros links:

- https://groups.google.com/forum/#!forum/osg-users
- http://www.openscenegraph.org/index.php/documentation/user-guides/57-osgdem
- https://www.osgvisual.org/wiki/OsgTerrainData
- http://docs.osgearth.org/en/latest/data.html
- https://github.com/openscenegraph/VirtualPlanetBuilder
- http://www.openscenegraph.org/index.php/documentation/tools/virtual-planet-builder
- http://www.andesengineering.com/BlueMarbleViewer/

E ainda os livros:

- Rui Wang & Xuelei Qian, 2010. OpenSceneGraph 3.0 Beginner's Guide: Create high-performance virtual reality applications with OpenSceneGraph, one of the best 3D graphics engines.
- Paul Martz, 2007. OpenSceneGraph Quick Start Guide: A Quick Introduction to the Cross-Platform Open Source Scene Graph API.
- Rui Wang & Xuelei Qian, 2012. OpenSceneGraph 3 Cookbook: Over 80 recipes to show advanced 3D programming techniques with the OpenSceneGraph API.

Os três livros podem ser encontrados na Library Genesis (http://libgen.is/).

---

#### Visualizando arquivos de relevo.

Encontrei [essa thread](https://groups.google.com/forum/#!searchin/osg-users/osgterrain$20data$20format|sort:date/osg-users/UoYOwKja4L8/n7OIZH3WCgAJ) que me deu a ideia de colocar .gdal no nome do arquivo.

`osgviewer --dem BRalt.tif.gdal`

gerou uma visualização 3D desta imagem:

![](BRalt.tif.png)

que ficou mais ou menos assim:

![](BRalt3D.png)

Detalhe: não precisei renomear o arquivo de BRalt.tif pra BRalt.tif.gdal.

Ainda não é o que precisamos, mas já é um passo na direção certa. Os próximos passos são:

- Ajustar a posição e movimento da câmera;
- Colocar o relevo na escala desejada;
- Acrescentar a textura (br.png);
- Fazer tudo isso (principalmente) via C++, e não (somente) pelo osgviewer.


---

#### Tentando com o [VirtualPlanetBuilder](https://github.com/openscenegraph/VirtualPlanetBuilder):

Baixei em ~/Downloads e descompactei. Depois:

```
$ cd ~/Downloads/VirtualPlanetBuilder-master
$ ./configure
$ cmake .
$ make
# make install
$ ls bin
osgdem  vpbcache  vpbmaster  vpbsizes
```

Não sei se o `cmake .` é realmente necessário. O executável mais importante foi criado, vpbmaster, que será usado no capítulo 7 do Cookbook.

