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
