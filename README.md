# L-DROID-G — protótipo Android

Versão de desenvolvimento 0.1.0. O projeto contém código-fonte Android para baixar um pacote `.ipk` por HTTPS e transferi-lo por SFTP para uma TV LG webOS na mesma rede, acionando o serviço de instalação em modo de desenvolvedor.

## Limitações importantes

- Este pacote é **código-fonte**, não um APK compilado. O ambiente de geração não possui Android SDK/Gradle instalados.
- O protótipo exige uma chave privada SSH válida e compatível com JSch. O pareamento automático pelo Key Server da LG ainda não está implementado.
- A aceitação da chave do host SSH não é verificada nesta versão inicial; use somente em sua rede doméstica confiável. Antes de uma versão de produção, deve ser implementada a verificação da impressão digital da TV.
- O método de instalação usa o serviço de desenvolvimento `com.webos.appInstallService/dev/install`, que pode variar conforme a versão do webOS e o estado do Developer Mode.
- O arquivo deve ser um pacote webOS `.ipk` compatível, não um APK Android. Aplicações Android não são executadas nativamente pelo webOS.
- O Developer Mode da LG tem sessão limitada; quando expirar, será necessário reativá-la/estendê-la na TV.

## Requisitos para compilar

- JDK 17 ou compatível com Android Gradle Plugin 8.7.3
- Android SDK Platform 35
- Gradle 8.9 ou compatível
- Acesso à Internet para baixar dependências Maven/Google

Abra a pasta no Android Studio e sincronize o Gradle. Para gerar o APK debug:

```bash
gradle assembleDebug
```

O arquivo resultante deverá ficar em `app/build/outputs/apk/debug/app-debug.apk`.

## Configuração da TV

1. Smartphone e TV na mesma rede Wi-Fi.
2. Na TV, abrir Developer Mode, ativar o modo de desenvolvedor e ligar o Key Server.
3. Usar o procedimento oficial da LG para obter e registrar a chave SSH. A configuração oficial usa IP da TV, porta `9922` e usuário `prisoner`.
4. No L-DROID-G, informar IP e URL HTTPS do `.ipk`, importar a chave privada autorizada e testar a conexão.
5. Tentar a instalação. Não use endereços IP de redes públicas nem exponha a porta 9922 à Internet.

Documentação:
- https://webostv.developer.lge.com/develop/getting-started/developer-mode-app
- https://webostv.developer.lge.com/develop/tools/cli-dev-guide


## Compilação automatizada (sem instalar Android Studio)

Foi incluído `.github/workflows/build-apk.yml`. Depois de colocar o projeto em um repositório GitHub:
1. Envie todos os arquivos do ZIP para um repositório.
2. Abra **Actions** e selecione **Build L-DROID-G APK**; execute **Run workflow** (ou envie código para `main`/`master`).
3. Ao terminar, abra a execução e baixe o artefato `L-DROID-G-debug-apk`.
4. Extraia `app-debug.apk` e instale-o no Android.

O artefato é um APK de depuração, não assinado para distribuição na Play Store. A compilação automatizada confirma apenas que o código compila; não substitui testes em uma TV LG real. Não insira chaves privadas no repositório.
