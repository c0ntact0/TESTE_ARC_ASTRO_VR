# Testes arcAstroVR (Source code)

Versão Unity: 6000.4.7f1  
Versão Stellarium: 26.2  
OS: MacOS Sequoia 15.7.7  
CPU: Apple M2 Pro  
RAM: 16 GB

## Preparação

- Criado um novo projeto Unity (URP) com o nome TESTE_ARC_ASTRO_VR;
- Download do repositório em https://github.com/arcAstroVR/arcAstroVR/tree/master;

A partir daqui foram usadas as instruções da secção **[Get & build the code](https://github.com/arcAstroVR/arcAstroVR/tree/master#get--build-the-code)** do README do repositório GitHub do arcAstroVR.

## Asset installation

- **Input System** (Unity Registry：Free): já vem instalada a versão 1.19.0;
- **Localization** (Unity Registry：Free): foi instalada a versão 1.5.12;
- **XR Interaction Toolkit** (Unity Registry：Free): foi instalada a versão 3.4.1;
- **[3rd Person Controller + Fly Mode](https://assetstore.unity.com/packages/templates/systems/3rd-person-controller-fly-mode-28647)** (Asset Store：Free): foi instalada e importada a versão 2.1.6. Existe uma [versão para dispositivos móveis](https://assetstore.unity.com/packages/templates/systems/3rd-person-controller-fly-mode-mobile-101764) com o custo de €13.79;
- **[JSON Object](https://assetstore.unity.com/packages/tools/input-management/json-object-710)** (Asset Store：Free): foi instalada e importada a versão 2.1.2;
- **[Dome Tools](https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/dome-tools-62664)**（Asset Store：Free): foi instalada e importada a versão 1.1;
- **[TriLib2](https://assetstore.unity.com/packages/tools/modeling/trilib-2-model-loading-package-157548)** (Asset Store：€41.40): para já não foi instalado devido ao custo;
- Importado o package `aAVR_0.16.1`.

Foi feito um restart ao Unity.

## Project settings

- Não existe Localization para `Portuguese`. Seria necessário criar uma para o projeto proposto;
- Parece existir um lapso no ponto 3. do README onde os autores referem:
  ```
  Click the Player in the left-hand list and select Localization  Settings from ActiveSettings.
  ```
  Não existe a opção `ActiveSettings` nas configurações do Player;
- No meu caso (MacOS) os pontos 4 e 5 não foram realizados.

## Erros de compilação

> Assets/TutorialInfo/Scripts/Readme.cs(4,14): error CS0101: The namespace '<global namespace>' already contains a definition for 'Readme'

A classe declarada no script `Assets/TutorialInfo/Scripts/Readme.cs` estava em conflito com a classe declarada em `Assets/3rdPerson+Fly/Scripts/Readme.cs` e esta última foi comentada.

---

> Assets/arcAstroVR/Script/aAV_Direction.cs(10,7): error CS0246: The type or namespace name 'TriLibCore' could not be found (are you missing a using directive or an assembly reference?)

> Assets/arcAstroVR/Script/aAV_FileSet.cs(496,22): error CS0246: The type or namespace name 'AssetLoaderContext' could not be found (are you missing a using directive or an assembly reference?)

> Assets/arcAstroVR/Script/aAV_FileSet.cs(24,10): error CS0246: The type or namespace name 'HumanoidAvatarMapper' could not be found (are you missing a using directive or an assembly reference?)

TODO: Existem vários erros destes derivados da não instalação do Asset TriLib. Ver o que fazer com os monitores.

---

> Assets/arcAstroVR/Script/aAV_Lines.cs(11,18): warning CS0108: 'aAV_Lines.camera' hides inherited member 'Component.camera'. Use the new keyword if hiding was intended.

Em versões antigas do Unity, `Component` tinha um membro chamado `camera`. A variável privada com o mesmo nome está a escondê-lo. A variável foi renomeada para `cameraPosition`.

# Testes arcAstroVR (App)

## Instalação

Foram seguidas as instruções da secção **[Installation Instructions & Quick Start](https://github.com/arcAstroVR/arcAstroVR/tree/master#installation-instructions--quick-start)** do README do repositório GitHub do arcAstroVR.

- Feito o download da versão para Mac (silicon);
- Feito o download do dataset **[Yoshinogari all area 2025](https://arcastrovr.org/download/data_yoshinogari2025.zip)**;
- No ponto 4-17. do README, os autores pedem para se mudar a path da `Screenshot Directory` para `/Users/<USERNAME>/Library/Application Support/Stellarium` (MacOS). No entanto, para ser menos intrusivo na configuração do Stellarium, criei um novo script para executar o Stellarium e configurar a variável de ambiente `STEL_SKYBOX_DIR`. Desta forma a configuração para os teste do Stellarium-Unity e do arcAstroVR permanecem as mesmas;
- O software funciona corretamente e faz a leitura dinâmica das Skyboxes;
- O sistema de datasets pareceu-me uma boa forma de generalizar as localizações.

# Teste terrain4aAVR/QGIS

## Observações ao manual

### Secção 5-1

- Os URLs corretos para usar na ligação XYZ Tiles para o GoogleMaps são:
  - Google Satellite: `https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}`;
  - Google Hybrid: `https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}`;
  - Google Maps: `https://mt1.google.com/vt/lyrs=m&x={x}&y={y}&z={z}`;
  - Google Terrain: `https://mt1.google.com/vt/lyrs=p&x={x}&y={y}&z={z}`.

Ver em [Alternativas de maps para textura](#alternativas-de-maps-para-texturas) como criar as ligações aos mapas.

Os links da Jaxa referidos no manual estão desatualizados. Pedi ajuda ao ChatGPT. Este sugeriu usar dados da Direção Geral do Território e Copernicus GLO-30.

#### Alternativas de maps para texturas

O Google Maps pode ser usado para criação de texturas, o manual refere o Google Maps (secção 5-5) como layer para criação de texturas tanto para terreno amplo (Wide Terrain) como para terreno local (Narrow Terrain). No entanto existem outras alternativas:

- Google Satellite: `https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}` (max. zoom 20);
- Esri World Imagery: `https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}` (max. zoom 20).

A ligação `XYZ Tiles` no QGIS deve ser feita da seguinte forma:

- Clicar com o botão do lado direito em `XYZ Tiles` no painel `Browser`;
- Escolher `New connection...`;
- Preencher:
  ```
  Name: um nome para a ligação
  URL: o URL para a o map
  Max. Zoom Level: normalmente 20
  ```

Para usar a ligação como uma layer deve arrastar a ligação para o painel `Layers`.

### Tentativa de criação de um _terrain_

Localização: `38.5575, -8.0614` (Cromeleque dos Almendres)

- Instalar o plugin **DGT CDD Downloader** no QGIS;
- Criar conta em https://cdd.dgterritorio.gov.pt/;
- No QGIS, em `Project -> Properties... -> CRS` selecionar:
  ```
  ETRS89 / Portugal TM06
  EPSG:3763
  ```
- No QGIS, em `Project -> Properties... -> General` confirmar:
  ```
  Units for distance: Meters
  Units for area: Square meters
  ```

#### Para descarregar o MDT local:

- No QGIS, `Processing -> Toolbox`;
- Procurar por `DGT CDD Portal`, expandir e clicar duas vezes em `DGT CDD Data Downloader`;
- O objetivo neste teste é o Cromeleque dos Almendres. Definir no mapa (QGIS) na layer do GoogleMaps,uma área de +/- 10 Km à volda do cromeleque;
  ![](images_readme/Screenshot%202026-07-28%20at%2014.56.23.png)
- Em `Area of Interest` escolher `Use Current Map Canvas Extend`;
- Em `Collections to download` escolher `MDT-2m`;

#### Ortofoto para textura local

Este passo é opcional, pode ser usado o Google Maps Satellite ou o Esri World Imagery para a textura local.

- Abrir o plugin `Terrain4aAVR` e configurar:
  ```
  Lat,Lng: 38.5575, -8.0614 (para Cromeleque dos Almendres, outro ponto se for esse ocaso)
  Em Narrow Terrain
  DEM Layer: DGT_MDT-2m
  Mesh (m): 2,0
  ```
  Não feche a janela do plugin. Na janela do mapa aparece uma caixa verde. Essa caixa é calculada pelo `Terrain4aAVR` e servirá de referência para a escolha da área da ortofoto;
- Ir a [https://www.dgterritorio.gov.pt/atividades/cartografia/cartografia-topografica/ortofotos/ortofotos-digitais](https://www.dgterritorio.gov.pt/atividades/cartografia/cartografia-topografica/ortofotos/ortofotos-digitais);
- Na linha `25 cm | 2018 | Portugal Continental` copiar o URL do `Serviço WMTS;
- No QGIS, ir a `Layer -> Add Layer -> Add WMS/WMTS Layer e depois `New`;
- Preencher com:
  ```
  Name: DGT Ortofotos 2018
  URL: endereço WMTS copiado da DGT
  ```
- Pressionar `OK` e depois `Connect`;
- Selecionar a layer `Ortos2018-RGB` e pressionar `Add`;
- Definir nas propriedades do projeto `CRS: EPSG:3763`;
- Fazer zoom à área dos Almendres;
- Em `Processing Toolbox -> Converte map to raster` configurar:
  ```
  Minimum extent to render: escolher uma caixa à volta do cromeleque usando a caixa verde como referência
  Units per pixel: 1,0 ou 0,5
  Layers to render: Ortos2018-RGB
  output layer: ortofoto_almendres.tif
  ```
  Em `Units per pixel`, para um local com 8,2 Km a relação é:
  - a 0,25 m: cerca de 32768 × 32768 pixels;
  - a 0,5 m: cerca de 16384 × 16384;
  - a 1 m: cerca de 8192 × 8192.

#### Preparar o DEM amplo

- Criar conta https://dataspace.copernicus.eu/ e selecionar `I am also interested in accessing Copernicus Contributing Missions data`;

- Entrar no [Copernico Browser](https://browser.dataspace.copernicus.eu/?zoom=16&lat=38.5575&lng=-8.0614&themeId=DEFAULT-THEME&visualizationUrl=U2FsdGVkX1%2FUV8Ogx0%2Bj5XzGfgPa%2FZ%2F3HuiGFUIV3tI1RyokZFAy45fQT4aGMP1yBLBO%2Bp8SMuE6e18UWrmujAQSrd%2BBL5fX3qzbacdXWoj4BSzExH6es3cK3qpjmcSI&datasetId=S2_L2A_CDAS&demSource3D=%22MAPZEN%22&cloudCoverage=30&dateMode=SINGLE), inserir as coordenadas do cromeleque na caixa de pesquisa (38.5575, -8.0614);
- No painel de pesquisa (do lado esquerdo), escolher `Copernicus DEM -> Global Coverage -> COP-DEM_GLO-30-DGED`;
- Fazer zoom até ter +/- 150 Km para cada lado a partir do cromeleque, i.e. uma extensão de +/- 300 Km;
- Mudar o ano inicial da pesquisa para 2001-01-01;
- Clicar em Search no fundo do painel;
- Para cada resultado fazer download do ficheiro ZIP. Em alternativa, pode clicar no "i" e na janela `PRODUCT INFO`, e em `Download single files` navega até à pasta `DEM` e faz download apenas do ficheiro TIFF;
- Extrair todos os ZIP para uma pasta;
- Copiar os TIFF localizados nas pastas `<pasta extraida do zip>/Copernicus_DSM_10_N38_00_W009_00/DEM` para uma outra pasta, e.g. pasta `rasters`;
- No QGIS, em `Layer -> Add Layer -> Add Raster Layer` selecionar todos os TIFF da pasta `rasters` e pressionar `Add`;
- No QGIS, em `Raster -> Miscellaneous -> Build Virtual Raster` configurar:
  ```
  Input layers: todos os GeoTIFF GLO-30
  Place each input file into a separate band: desativado
  Resolution: Average
  Output: almendres_glo30.vrt
  ```
  ![Aspeto do VTR](images_readme/Screenshot%202026-07-29%20at%2014.16.58.png)
- Para converter para GeoTIFF (compatível com **terrain4aAVR**), no QGIS, em `Raster -> Conversion -> Translate (Convert Format)` configurar:
  ```
  Input layer: almendres_glo30.vrt
  Output file: almendres_glo30_merged.tif
  ```
  Nas opções de criação, em `Advanced Parameters -> Additional creation options` adicionar:
  ```
  COMPRESS=DEFLATE
  TILED=YES
  BIGTIFF=IF_SAFER
  ```

#### Criar _Geoid_

A área de cobertura que interessa para o geoid é o terreno amplo. Se a cobertura do geoid incluí outros terrenos amplos que se criem, não é necessário criar um novo geoid.

Para áreas que cubram apenas o território português pode se usado `GeodPT08` que é específico para Portugal.

Para áreas que cubram outros territórios, como o oceano e Espanha, deve ser usado um geoid global, algo como o `EGM2008 1'`.

Se o território coberto por outros

- Entrar em [https://www.agisoft.com/downloads/geoids/](https://www.agisoft.com/downloads/geoids/);
- Para portugal:
  Na secção **Portugal**, fazer download do [GeodPT08 geoid model](https://download.agisoft.com/gtg/pt_dgt_GeodPT08.tif);
- Global:
  Na secção **Global Geoid Models**, fazer download do [EGM2008 1' geoid model](https://download.agisoft.com/gtg/us_nga_egm2008_1.tif);

- No QGIS, `Layer -> Add Layer -> Add Raster Layers`e selecionar o ficheiro descarregado;

#### Criar **mask**

A mask é uma layer vetorial de polígonos que indica ao terrain4aAVR onde deve baixar o terreno para evitar interseção com o modelo 3D.

Só deve ser criada depois de se saber qual é a área de terreno incluída no modelo Sketchfab.

- No QGIS, `Layer -> Create Layer -> New GeoPackage Layer`;
- Configurar:
  ```
  Database: almendres.gpkg
  Table name: mask_almendres
  Geometry type: Polygon
  CRS: EPSG:3763
  ```
  Pressionar OK;
- Usar a ortofoto (ou o Google Maps ou o Esri World Imagery) como fundo e navegar até ao cromeleque;
- Selecionar a layer `mask_almendres`;
- Ativar `Toggle Editing` (o lápis na toolbar);
- Clicar em `Add Polygon Feature` (terceiro ícone depois do lápis);
- Desenhar um poligono em da área ocupada pelo modelo 3D. pode usar o preview do modelo no sketchfab como referência;
- Usar o botão do lado direito para terminar a edição;
- Gravar clicando na diskette ao lado do lápis e fazer `Toggle Editing`;

#### Criar terreno com terrain4aAVR

##### Correção de erro (obrigatório)

A versão 3.44.12-Solothurn do QGIS dá este erro no final do processamento da versão atual do terrain4aAVR:

```
An error has occurred while executing Python code:
TypeError: arguments did not match any overloaded call:   QSize(): too many arguments   QSize(w:  h:    int): argument 1 has unexpected type 'float'   QSize(a0: QSize): argument 1 has pected type 'float'
Traceback (most recent call last):
  File "/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/ain4aAVR/    terrain4aAVR.py", line 347, in run
    self.main()
  File "/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/ain4aAVR/    terrain4aAVR.py", line 882, in main
    options.setOutputSize(QSize(self.L_dem_px / 3, self.L_dem_px / 3))
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: arguments did not match any overloaded call:
  QSize(): too many arguments
  QSize(w: int, h: int): argument 1 has unexpected type 'float'
  QSize(a0: QSize): argument 1 has unexpected type 'float'
Python version: 3.12.11 (main, Jul  3 2026, 15:57:42) [Clang 17.0.0 (clang-1700.6.4.2)]
QGIS version: 3.44.12-Solothurn Solothurn, 32c5dba7
Python Path:
/Applications/QGIS.app/Contents/Resources/qgis/python
/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python
/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins
/Applications/QGIS.app/Contents/Resources/qgis/python/plugins
/Applications/QGIS.app/Contents/Frameworks/lib/python312.zip
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12/lib-dynload
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12/site-packages
/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python
/Users/user/SynologyDrive/MEIM_OFFLINE/S4/TFM
```

Este erro deve-se a divisões que resultam em `float`. Esses valores estão a ser usados em funções `QSize` que só aceitam `int`. Estes erros aparecem na versão 3.44.12-Solothurn do QGIS devido à versão da workframe do QT que é usada. **Este problema deve ser resolvido**, pois impede a geração dos ficheiros JPG do terreno amplo.

Resolver o erro:

- Cópia de segurança:
  ```console
  cp "/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/ain4aAVR/terrain4aAVR.py" "/Users/user/Library/  Application Support/QGIS/QGIS3/profiles/default/python/plugins/terrain4aAVR/ain4aAVR.py.bak"
  ```
- Editar `/Users/user/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/ain4aAVR/terrain4aAVR.py`
- Na linha 882 alterar
  ```python
  options.setOutputSize(QSize(self.L_dem_px / 3, self.L_dem_px / 3))
  ```
  Para
  ```python
  wide_texture_px = self.L_dem_px // 3
  options.setOutputSize(QSize(wide_texture_px, wide_texture_px))
  ```
- Procurar todas as ocorrências de `QSize(`
- Ao encontrar
  ```python
  options.setOutputSize(QSize(pxsize, pxsize))
  ```
  Substituir a secção toda por
  ```python
  pxsize = self.S_output_size / self.S_tex_mesh
  if pxsize > 16384:
      pxsize = 16384
  pxsize = int(round(pxsize)) # new
  texlayer = QgsProject.instance().mapLayersByName(
      "narrowTerrain_TEX"
  )[0]
  options = QgsMapSettings()
  options.setLayers([texlayer])
  options.setBackgroundColor(QColor(0, 0, 0))
  options.setOutputSize(QSize(pxsize, pxsize))
  ```
- Procurar por TILE_SIZE
  Ao encontrar
  ```python
  parameters = {'EXTENT': area_rect, 'LAYERS': [uri], 'MAP_UNITS_PER_PIXEL': tex_mesh,
                    'OUTPUT': 'TEMPORARY_OUTPUT', 'TILE_SIZE': width / tex_mesh}
  ```
  Substituir por
  ```python
  tile_size = int(round(width / tex_mesh))
  parameters = {'EXTENT': area_rect, 'LAYERS': [uri], 'MAP_UNITS_PER_PIXEL': tex_mesh,
                    'OUTPUT': 'TEMPORARY_OUTPUT', 'TILE_SIZE': tile_size}
  ```

O QGIS deve ser reiniciado para que as modificações tenham efeito.

##### Outras modificações ao código (opcional)

- Por omissão a depressão causada pela _mask_ no terreno é de 3 metros. Este valor é muito alto e gera fossos profundos em situações em que o modelo não pode ser posicionado de forma a cobrir esses fossos. Uma forma de contornar este problema é gerar _masks_ mais pequenas, o que pode não ser possível se o modelo contém objetos importantes nas extremidades. Outra forma é reduzir a profundidade da depressão. Isso pode ser alcançado com a variável `self.down`:
  Em vez de
  `python
self.down: float = 3
`
  Usar, por exemplo
  `python
self.down: float = 1
`
- Na linha
  ```python
  self.dlg.mQgsFileWidget_outputPath.setFilePath(os.path.expanduser('~/Desktop'))
  ```
  O caminho `~/Desktop` pode ser substituido por outro. Assim evita-se estar sempre a mudar o caminho quando se chama o plugin.

O QGIS deve ser reiniciado para que as modificações tenham efeito.

##### Criar terreno

- No `terrain4aAVR` use a seguinte configuração:

  ```
  Lat, Lng: 38.5575,-8.0614

  Wide Terrain
  DEM layer: wide_almendres_glo30_merged
  Geoid layer: us_nga_egm2008_1
  Texture layer: GoggleMaps (Satellite)

  Narrow Terrain
  DEM layer: DGT_MDT-2m
  Geoid layer: us_nga_egm2008_1
  Texture layer: ortofoto_almendres
  Mesh (m): 2,0

  Mask Layer: mask_almendres (Fill Value entre 5-2 metros. Por omissão são 5 metros)
  ```

- Pressione OK para gerar o terreno. Este processo demora algum tempo.

Na textura do Narrow Terrain também pode ser usado o Google Maps. Neste caso em concreto do Cromeleque dos Almendres, existe uma mudança de tonalidade que não fica dada bem. Também pode ser usado o Esri World Imagery para as duas texturas.

#### Preencher dataset.txt

Para saber o valor de altitude de uma layer, e.g. MDT, dev:

1. Selecionar `Identify Feature` no QGIS (ícone com um i na barra de ferramentas superior);
1. Selecionar a layer pretendida e clicar no local do mapa que pretende saber a altitude;
1. Na caixa `Identify Results`(lado esquerdo), o valor do campo `Band 1` é a altitude. E.g. para o Cromleque dos Almendres, o valor MDT é $ \approxeq 412,3187$.

- center
  - Primeiro longitude e depois latitude
  - O terceiro valor é a altura elipsoidal. Se não for usado layer geoid, este valor é a o valor da altitude do layer MDT, e.g. `DGT_MDT-2m`.
  - Se foi usado geoid o valor é:
    $$
        h=H+N
    $$
    Em que $H$ é o valor da altura MDT e $N$ é o valor da altura da layer geoid. No caso do cromeleque dos Almendres os valores são:
    $$412,32 + 55,38 = 467,7$$

## Exportar modelo para FBX

No Blender:

- Importar o GLB.
- Selecionar todas as partes do modelo, em Scene Collection, botão do lado direito e `Select hierarchy`;
- Object -> Set Origin -> Origin to Geometry;
- Pode usar `Numpad 7`para top view e `Numpad 3`para vista lateral. `Numpad 5` alterna entre vista ortografica e vista de perspetiva;
- Colocar o modelo próximo de:
  ```
  X = 0
  Y = 0
  Z = 0
  ```
- Aplicar as transformações, `Object -> Apply -> All Transforms`
- Confirmar que:

  ```
  Scale = 1,1,1
  Rotation = 0,0,0
  ```

- Exporta para FBX, usando unidades métricas.
  No Blender, o eixo vertical é Z; no Unity é Y. Na exportação FBX, normalmente deve usar:
  ```
  Selected Objects: ativo
  Apply Transform: ativo
  Forward: -Z Forward
  Up: Y Up
  Apply Scalings: FBX Units Scale
  Path Mode: Copy
  Embed Textures: ativo
  ```
- Deverá ser necessário fazer ajustes depois do objeto importado para o `arcAstroVR`.

# Localizações de monumentos

Cromeleque dos Almendres: `38.5575,-8.0614`
Cromeleque da Portela de Mogos: `38.62645627525851, -8.025696353431758`

# Orientação dos monumentos

Quando os monumentos estão cobertos por estruturas artificiais ou naturais, torna-se muito difícil orientar e escalar os modelos 3D usando somente imagens de satélite. Para orientar e escalar os modelos de forma precisa, é necessário obter dados no terreno ou documentação que possa ser usada para esse fim. Para já, tenho pesquisado algumas fontes que podem ser úteis para este trabalho. A orientação e escala do Cromeleque da Portela de Mogos, é impossível através de imagens de satélite, pois o conjunto megalitico encontra-se localizado entre sobreiros, cujas as copas ocultam o monumento visto de cima. Documento aqui alguns links que encontrei e que podem ser úteis, não só para resolver este problema sem ser necessário ir ao local, como para estudo posterior:

- [The Megalithic Portal](https://www.megalithic.co.uk/article.php?sid=14608)
- [Mário Varela Gomes](https://fcsh-unl.academia.edu/MarioVarelaGomes)
- [Prehistoric Portugal](https://prehistoricportugal.com/cromeleque-da-portela-de-mogos/)

O vídeo que pode ser visto em [The Megalithic Portal](https://www.megalithic.co.uk/article.php?sid=14608) e em [Prehistoric Portugal](https://prehistoricportugal.com/cromeleque-da-portela-de-mogos/) tem uma versão do diagrama do Cromeleque da Portela de Mogos, criado por Mário Varela Gomes, com a orientação do monumento:
![](images_readme/Screenshot%202026-08-07%20at%2017.31.02.png)

A escala também é representada, o que ajuda no escalamento do modelo.

WORK IN PROGRESS

---

O texto a seguir é uma saída do ChatGPT para alinhar a planta no terreno de forma a criar um referencial para alinhamento e escalamento do modelo.

## 1. Criar `planta_alinhada`

Abre:

**Processing Toolbox → GDAL → Raster projections → Warp (reproject)**

Configura:

**Input layer**

```text
planta
```

**Source CRS**

Deixa em branco / não alteres.

**Target CRS**

Deixa em branco / não alteres.

Não queremos transformar coordenadas, apenas resamplear para a grelha de `mogos`.

**Resampling method**

```text
Nearest neighbour
```

Para este desenho é a opção que menos altera os pixels originais.

**Output file resolution in target georeferenced units**

```text
0.25
```

**Georeferenced extents of output file**

Introduz exatamente:

```text
-4517.6174,-4708.8691,4698.3826,4507.1309
```

ou, ainda melhor, usa o botão:

**Calculate from Layer → mogos**

O `Warp` permite fixar explicitamente resolução e extensão de saída. ([QGIS Documentação][1])

Guarda:

```text
planta_alinhada.tif
```

Não definas NoData nesta fase.

---

## 2. Criar `alpha_alinhado`

Repete a mesma operação mas com `Addicional command-line parameters` de forma a colocar a 0 tudo o que está fora da planata original:

**Input layer**

```text
planta_alpha_calc
```

Configuração:

```text
Resampling = Nearest neighbour
Resolution = 0.25
Extent = mogos
Addicional command-line parameters = -wo INIT_DEST=0
```

Output:

```text
alpha_alinhado.tif
```

Também sem Target CRS.

---

## 3. Verificar dimensões

Agora abre:

**Properties → Information**

para os dois.

Devem ficar exatamente:

```text
Width:  36864
Height: 36864
```

e:

```text
Pixel Size:
0.25,-0.25
```

Extent:

```text
-4517.6174,-4708.8691 : 4698.3826,4507.1309
```

Neste ponto temos:

```text
mogos             36864 × 36864
planta_alinhada   36864 × 36864
alpha_alinhado    36864 × 36864
```

Esse é o requisito crítico antes de qualquer Raster Calculator.

### Um ponto sobre o `CRS: unknown`

Isto não impede necessariamente esta composição, porque estamos a trabalhar num sistema cartesiano local e os três rasters já estão posicionados nesse mesmo espaço. Mas convém, mais tarde, perceber **que sistema de coordenadas corresponde ao `mogos`**, sobretudo se pretendes integrar estas coordenadas com QGIS, GPS ou ETRS89/PT-TM06.

Neste momento não mudaria o CRS, porque atribuir um CRS errado deslocaria completamente os dados.

Faz primeiro até ao **passo 4**. Se `planta_alinhada` e `alpha_alinhado` aparecerem agora como `36864 × 36864`, resolvemos a principal fonte dos problemas anteriores. Depois tratamos apenas o NoData e fazemos a composição RGB.

[1]: https://docs.qgis.org/3.44/hu/docs/user_manual/processing_algs/gdal/rasterprojections.html?utm_source=chatgpt.com "24.2.5. Raster projections — QGIS Documentation dokumentáció"
[2]: https://gdal.org/en/stable/programs/gdalwarp.html?utm_source=chatgpt.com "gdalwarp — GDAL documentation"

---

Agora que `planta_alinhada`, `planta_alpha_calc_alinhada` e `mogos` têm exatamente as mesmas dimensões, o processo fica simples.

Assumindo:

- `planta_alinhada` = RGB
- `mogos` = RGB
- `planta_alpha_calc_alinhada` = 1 banda, com `0` no fundo e `255` no desenho

faz três canais finais no **Raster Calculator**.

Para o vermelho:

```text
("planta_alpha_calc_alinhada@1" / 255.0) * "planta_alinhada@1"
+
(1 - ("planta_alpha_calc_alinhada@1" / 255.0)) * "mogos@1"
```

Guarda como `final_R.tif`.

Para o verde:

```text
("planta_alpha_calc_alinhada@1" / 255.0) * "planta_alinhada@2"
+
(1 - ("planta_alpha_calc_alinhada@1" / 255.0)) * "mogos@2"
```

Guarda como `final_G.tif`.

Para o azul:

```text
("planta_alpha_calc_alinhada@1" / 255.0) * "planta_alinhada@3"
+
(1 - ("planta_alpha_calc_alinhada@1" / 255.0)) * "mogos@3"
```

Guarda como `final_B.tif`.

A lógica é:

$$
C_{final}=\alpha C_{planta}+(1-\alpha)C_{mogos}
$$

com $\alpha=\frac{\text{máscara}}{255}$.

Depois junta os três canais:

**Processing Toolbox → GDAL → Raster miscellaneous → Build Virtual Raster**

Inputs, nesta ordem:

```text
final_R.tif
final_G.tif
final_B.tif
```

Ativa:

**Place each input file into a separate band**

Guarda como:

```text
mogos_com_planta.vrt
```

O VRT deverá ficar com:

```text
Band 1 = final_R
Band 2 = final_G
Band 3 = final_B
```

Depois abre `mogos_com_planta.vrt` no QGIS e confirma em **Properties → Information**:

```text
Band count: 3
```

e verifica se os mínimos/máximos são agora semelhantes aos dos `final_R/G/B`, e não zero.

Se estiver correto, converte o VRT para GeoTIFF:

**Processing Toolbox → GDAL → Raster conversion → Translate (Convert format)**

Input:

```text
mogos_com_planta.vrt
```

Output:

```text
mogos_com_planta_final.tif
```

Em **Output data type**, escolhe:

```text
Byte
```

e em **Additional command-line parameters** podes colocar:

```text
-colorinterp red,green,blue
```

O `gdal_translate` permite definir a interpretação de cada banda; além disso, um GeoTIFF Byte com exatamente três bandas é normalmente tratado pelo driver GTiff como RGB. ([gdal.org][2])

No fim, em **Properties → Symbology**:

```text
Render type: Multiband color

Red band:   Band 1
Green band: Band 2
Blue band:  Band 3
```

Este é agora o caminho que eu usaria:

```text
final_R ─┐
final_G ─┼─ Build VRT (separate) → mogos_com_planta.vrt
final_B ─┘                              │
                                       ↓
                                  Translate
                                       │
                                       ↓
                            mogos_com_planta_final.tif
                                  RGB, 3 bandas
```

Não uses mais o `Merge` para juntar estes três canais; no teu caso concreto está claramente a escrever zeros no output.

[1]: https://gdal.org/en/stable/programs/gdalbuildvrt.html?utm_source=chatgpt.com "gdalbuildvrt — GDAL documentation"
[2]: https://gdal.org/en/stable/programs/gdal_translate.html?utm_source=chatgpt.com "gdal_translate — GDAL documentation"
