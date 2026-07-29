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
- Os URLs corretos para usar na conexao XYZ Tiles para o GoogleMaps são:
    - Google Satellite: `https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}`;
    - Google Hybrid: `https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}`;
    - Google Maps: `https://mt1.google.com/vt/lyrs=m&x={x}&y={y}&z={z}`;
    - Google Terrain: `https://mt1.google.com/vt/lyrs=p&x={x}&y={y}&z={z}`.

Os links da Jaxa referidos no manual estão desatualizados. Pedi ajuda ao ChatGPT. Este sugeriu usar dados da Direção Geral do Território e Copernicus GLO-30.

### Tentativa de criação de um *terrain*

Localização: `38.5575, -8.0614` (Cromeleque dos Almendres)

- Instalei o plugin **DGT CDD Downloader** no QGIS;
- Criei conta em https://cdd.dgterritorio.gov.pt/;
- No QGIS, em `Project -> Properties... -> CRS` selecionei:
    ```
    ETRS89 / Portugal TM06
    EPSG:3763
    ```
- No QGIS, em `Project -> Properties... -> General` confirmei:
    ```
    Units for distance: Meters
    Units for area: Square meters
    ```
#### Para descarregar o MDT local:
    
- No QGIS, `Processing -> Toolbox`;
- Procurar por `DGT CDD Portal`, expandir e clicar duas vezes em `DGT CDD Data Downloader`;
- O objetivo neste teste é o Cromeleque dos Almendres. Defini no mapa (QGIS) na layer do GoogleMaps,uma área de +/- 10 Km à volda do cromeleque;
![](images_readme/Screenshot%202026-07-28%20at%2014.56.23.png)
- Em `Area of Interest`escolhi `Use Current Map Canvas Extend`;
- Em `Collections to download` escolhi `MDT-2m`;
#### Preparar o DEM amplo
- Criei conta https://dataspace.copernicus.eu/ e selecionei `I am also interested in accessing Copernicus Contributing Missions data`;
- Entrei no [Copernico Browser](https://browser.dataspace.copernicus.eu/?zoom=16&lat=38.5575&lng=-8.0614&themeId=DEFAULT-THEME&visualizationUrl=U2FsdGVkX1%2FUV8Ogx0%2Bj5XzGfgPa%2FZ%2F3HuiGFUIV3tI1RyokZFAy45fQT4aGMP1yBLBO%2Bp8SMuE6e18UWrmujAQSrd%2BBL5fX3qzbacdXWoj4BSzExH6es3cK3qpjmcSI&datasetId=S2_L2A_CDAS&demSource3D=%22MAPZEN%22&cloudCoverage=30&dateMode=SINGLE), inseri as coordenadas do cromeleque na caixa de pesquisa (38.5575, -8.0614);
- No painel de pesquisa (do lado esquerdo), escolhi `Copernicus DEM -> Global Coverage -> COP-DEM_GLO-30-DGED`; 
- Fiz zoom até ter +/- 150 Km para cada lado a partir do cromeleque, i.e. uma extensão de +/- 300 Km;
- Mudei o ano inicial da pesquisa para 2001-01-01;
- Cliquei em Search no fundo do painel;
- Para cada resultado fiz download do ficheiro ZIP;
- Descompactei todos os ZIP para uma pasta;
- Copiei os TIFF localizados nas pastas `<pasta extraida do zip>/Copernicus_DSM_10_N38_00_W009_00/DEM` para uma outra pasta, e.g. pasta `rasters`;
- No QGIS, em `Layer -> Add Layer -> Add Raster Layer` selecionei todos os TIFF da pasta `rasters` e pressionei `Add`;
- No QGIS, em `Raster -> Miscellaneous -> Build Virtual Raster` configurei:
    ```
    Input layers: todos os GeoTIFF GLO-30
    Place each input file into a separate band: desativado
    Resolution: Average
    Output: almendres_glo30.vrt
    ```
    ![Aspeto do VTR](images_readme/Screenshot%202026-07-29%20at%2014.16.58.png)
- Para converter para GeoTIFF (compatível com **terrain4aAVR**), no QGIS, em `Raster -> Conversion -> Translate (Convert Format)` configurei:
    ```
    Input layer: almendres_glo30.vrt
    Output file: almendres_glo30_merged.tif
    ```
    Nas opções de criação, em `Advanced Parameters -> Additional creation options`adicionei:
    ```
    COMPRESS=DEFLATE
    TILED=YES
    BIGTIFF=IF_SAFER
    ``` 
#### Preparar texturas
- Usar apenas o Google Maps mostrou alguns problemas, nomeadamente *tiles* no mar que ficavam abranco:
![](images_readme/Screenshot%202026-07-29%20at%2019.31.18.png)
E também diferenças de coloração do terreno entre mosaicos;
- Foi decido usar o Copernicus Sentinal-2 Mosaic. No Copernico Browser, painel de pesquisa, escolher`Sentinel Mosaics -> Sentinel-2 -> Quarterly Mosaics`;
- Escolher 2001-01-01 como data de inicio da pesquisa e pressionar e clicar em Search;
- Ir para `VISUALIZE`, selecionar a layer `True Color Cloudless`;
- Na toolbox do lado direito clicar o icone do pentágono e criar uma área de interesse que cubra oquadrado vermelho do QGIS;
![](images_readme/Screenshot%202026-07-29%20at%2020.37.37.png)
- Na toolbox pressione o botão de download;
- Vá para o separador `Analytical` e preencha com a configuração da imagem abaixo;
![](images_readme/Screenshot%202026-07-29%20at%2020.40.44.png)
- Pressione `DOWNLOAD`para guardar a imagem no disco;
- Arraste o TIFF para o quadro de layers do QGIS.


#### Criar terreno com terrain4aAVR
- No `terrain4aAVR` use a seguinte configuração:
    ![](images_readme/Screenshot%202026-07-29%20at%2020.57.31.png)
    Que corresponde aos seguintes layers:
    ![](images_readme/Screenshot%202026-07-29%20at%2020.55.49.png)

An error has occurred while executing Python code: 

TypeError: arguments did not match any overloaded call:   QSize(): too many arguments   QSize(w: int, h: int): argument 1 has unexpected type 'float'   QSize(a0: QSize): argument 1 has unexpected type 'float' 
Traceback (most recent call last):
  File "/Users/rui/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/terrain4aAVR/terrain4aAVR.py", line 347, in run
    self.main()
  File "/Users/rui/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins/terrain4aAVR/terrain4aAVR.py", line 882, in main
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
/Users/rui/Library/Application Support/QGIS/QGIS3/profiles/default/python
/Users/rui/Library/Application Support/QGIS/QGIS3/profiles/default/python/plugins
/Applications/QGIS.app/Contents/Resources/qgis/python/plugins
/Applications/QGIS.app/Contents/Frameworks/lib/python312.zip
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12/lib-dynload
/Applications/QGIS.app/Contents/Frameworks/lib/python3.12/site-packages
/Users/rui/Library/Application Support/QGIS/QGIS3/profiles/default/python
/Users/rui/SynologyDrive/MEIM_OFFLINE/S4/TFM

WORK IN PROGRESS



