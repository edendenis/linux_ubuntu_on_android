# Como configurar/instalar/usar o `Linux Ubuntu` no `Android`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar/usar o `Linux Ubuntu` no `Android`.

## _Abstract_

_This document contains the main commands and settings for configuring/installing/using `Linux Ubuntu` on `Android`._


## Revisão(ões)/Versão(ões)

| Revisão número | Data da revisão | Descrição da revisão                                    | Autor da revisão                                |
|:--------------:|:---------------:|:--------------------------------------------------------|:------------------------------------------------|
| 0              | 27/03/2024      | <ul><li>Revisão inicial/criação do documento.</li></ul> | <ul><li>Eden Denis F. da S. L. Santos</li></ul> |


## Descrição [2]

### `Linux Ubuntu`

O Linux Ubuntu é uma distribuição de sistema operacional baseada em Linux conhecida por sua facilidade de uso e robustez. Construído sobre a base do Debian, o Ubuntu oferece uma ampla gama de aplicativos e ferramentas pré-instaladas, além de uma vasta comunidade de usuários e desenvolvedores. Seu ambiente de desktop padrão, o GNOME, proporciona uma experiência intuitiva e personalizável para os usuários. O Ubuntu é amplamente utilizado em ambientes pessoais e empresariais devido à sua estabilidade, segurança e suporte de longo prazo.

### `Android`

Android é um sistema operacional móvel desenvolvido pelo Google, conhecido por sua ampla adoção em dispositivos como smartphones, tablets, smartwatches e TVs inteligentes. Baseado no kernel do Linux, o Android oferece uma plataforma flexível e personalizável para desenvolvimento de aplicativos, suportando uma vasta gama de recursos, desde comunicações e entretenimento até produtividade e saúde. Com a Google Play Store como sua principal fonte de aplicativos, o Android oferece aos usuários uma variedade de opções para personalizar e expandir suas experiências móveis.

Aqui está o passo a passo para usar o seu dispositivo `Android` como um `PC`, executando uma distribuição `Linux` diretamente do `Termux`:

1. **Instalação do Termux:** Baixe e instale o `Termux` da `Play Store` ou através do `F-Droid`.

2. Abra o `Termux` e atualize os pacotes digitando o comando: `pkg update -y`

2. **Instalação do Proot e Proot-Distro:** Instale o Proot-Distro digitando o comando: `pkg install proot-distro -y`

3. **Consultar as distribuições disponíveis para instalação:** Para consultar as distribuições disponíveis para instalação digite o comando: `proot-distro list`

4.  Verifique se a distribuição que você deseja está disponível na lista

5. **Instalação da Distribuição `Linux` (`Ubuntu`):** Instale a distribuição desejada, por exemplo, `Ubuntu`, digitando o comando específico. No exemplo, usaremos o `Ubuntu 23.04`, mas o comando pode ser finalizado com apenas o `ubuntu` sem especificar a versão: `proot-distro install ubuntu`

6. **Atualização e Configuração da Distribuição:** Após a instalação, acesse a distribuição `Linux` digitando: `proot-distro login ubuntu`

7. **Atualize os pacotes com:** `apt update -y && apt full-upgrade -y`

8. **Instalação da Interface Gráfica (`xfce`):** Instale a interface gráfica `xfce` digitando o comando: `apt install xfwm4 xfce4-panel xfce4-settings xfce4-session xfce4-terminal xfdesktop4 xfce4-taskmanager thunar dbus-x11 elementary-xfce-icon-theme --no-install-recommends -y`

9. **Configurar a região**: `Please select the geographic area in which you like. Subsequent configuration questions will narrow this down by presenting a lis of cities, representing the time zones in chiwch they are located`. **Cuidado**, pois o número pode mudar. Escolher a opção: `2. America`

10. **Configurar a cidade:** `Please select the city or region corresponding to your time zone` **Cuidado**, pois o número pode mudar. Escolher a opção: `135. Sao Paulo`

11. **Instalação do Servidor `VNC` (TigerVNC):** Instale o servidor `VNC` digitando: `apt install tigervnc-standalone-server -y`

12. **Instalar o `xterm`:** Se o `xterm` não estiver instalado, você pode instalá-lo usando o gerenciador de pacotes da sua distribuição Linux. Para a maioria das distribuições baseadas em Debian (como Ubuntu), você usaria: `apt install xterm -y`

13. **Inicie o servidor VNC utilizando o comando:** `vncserver -geometry 1440x720`

14. **Definir uma senha para acessar seus `desktops`** `You will require a password to access your desktops`

15. **Would you like to enter a view-only password (y/n)?:** Escola a opção `yes`: `y`

16. No arquivo `nano ~/.vnc/xstartup` ou (mais provavél) `/data/data/com.termux/files/home/.vnc/xstart`, adicione a linha `exec startxfce4` e salve o arquivo. Se **NÂO** existir, crie o arquivo o qual deve ficar com o seguinte conteúdo:

    ```
    #!/bin/sh
    exec startxfce4
    test x"$SHELL" != x"" && SHELL=/bin/bash
    test x"$1" = x"set" -- default

    vncconfig -iconic &
    $SHELL -l <<EOF
    exec /etc/X11/Xsession "$@"
    EOF

    vncserver -kill $DISPLAY
    ```

    ou

    ```
    ## This file is executed during VNC server
    ## startup.
    exec startxfce4

    # Launch terminal emulator Aterm.
    # Requires package 'aterm'.
    aterm -geometry 80x24+10+10 -ls &

    # Launch Tab Window Manager.
    # Requires package 'xorg-twm'.
    twm &
    ```

17. **Torne o xstartup Executável:** Se o arquivo `xstartup` não for executável, você pode torná-lo executável com o seguinte comando: `chmod +x /root/.vnc/xstartup`

18. Encerrar a sessão do `VNC` para iniciar a que irá executar o `Ubuntu`: `vncserver -kill :1`

    18.1 Você pode tentar remover manualmente os arquivos de bloqueio e soquete do servidor X para o display `:1` conforme sugerido nos avisos:

    ```
    rm /tmp/.X11-unix/X1
    rm /tmp/.X1-lock
    ```

19. **Iniciar uma nova sessão do `VNC`:** `vncserver -geometry 1440x720`

20. **Acesso à Interface Gráfica:**

21. Baixe e instale um cliente `VNC` em seu dispositivo `Android` (por exemplo, `VNC Viewer`).
 
22. Crie uma nova conexão no `VNC Viewer`, especificando o endereço `localhost:5901`

    O último número `1`, é o número do display que o `vncserver` criou anteriormente onde está indicado `:1`.

23. Conecte-se e insira a senha definida durante a configuração do `VNC`.

24. **Criação de um Novo Usuário:** Crie um novo usuário no `Linux` para evitar problemas de segurança: `adduser edenedfsls`

35. Adicione o novo usuário ao grupo `sudoers` para conceder privilégios de superusuário: `usermod -aG sudo edenedfsls`

36. **Instalar o `sudo`:** `apt install sudo -y`

37. **Definir uma senha para o usuário criado `edenedfsls`:** `passwd edenedfsls`

38. **Adicionar o novo usuário criado `edenedfsls` para o grupo `sudoers`:**

    ```
    EDITOR=nano
    export EDITOR=nano
    visudo
    ```

39. **Trocar de usuário:** `su - edenedfsls`

40. **Trocar o `shell` padrão do novo usuário criado `edenedfsls`:** `chsh`

41. **Digitar o caminho para o bash que deseja:** `/bin/bash`

42. **Desligamento e Reinicialização do Sistema:**

43. Para desligar o sistema, encerre todas as sessões `VNC` com `vncserver -kill :1` e saia da sessão `Linux`.

44. **Trocar de usuário antes de iniciar o `vncserver`:** `su - edenedfsls`

45. **Criar os arquivos:**

    ```
    touch /home/edenedfsls/.Xauthority
    chmod 600 /home/edenedfsls/.Xauthority
    ```

45. Para reiniciar o sistema, execute os comandos de inicialização do `VNC` e da sessão `Linux` novamente: `tigervncserver -xstartup /usr/bin/xterm`

Conecte-se ao endereço e porta especificados durante a configuração do servidor `VNC` em seu dispositivo `Android`.

Agora você pode usar seu dispositivo `Android` como um `PC`, executando uma distribuição Linux completa e acessando uma interface gráfica via `VNC`. Lembre-se de adaptar os comandos e configurações conforme necessário e aproveite a experiência!

## 2. Configuração do Áudio (opcional): Instale o `PulseAudio` no `Termux` e na distribuição `Linux`.

1. **No `Termux`:** Instale o `PulseAudio`: `pkg install pulseaudio -y`

2. Execute o comando abaixo para criar o arquivo de configuração `default.pa` dentro do diretório `.config/pulse/`:

    ```
    mkdir -p ~/.config/pulse
    nano ~/.config/pulse/default.pa
    ```

    Se o arquivo já existir, você será levado para editá-lo. Caso contrário, um novo arquivo será criado e você será levado para adicioná-lo.

    2.1 **Adicione a seguinte linha ao arquivo `default.pa`:** `load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;192.168.0.0/16 auth-anonymous=1`

    Após adicionar a linha, salve o arquivo pressionando `Ctrl + O`, confirme com `Enter`, e depois saia do editor pressionando `Ctrl + X`.

    Com isso, você configurou o `PulseAudio` para iniciar automaticamente e permitiu conexões apenas do `localhost` e da rede local.

    2.2 Para desligar o sistema, encerre todas as sessões `VNC` com `vncserver -kill :1` e saia da sessão `Linux`.

    2.3 Você pode tentar remover manualmente os arquivos de bloqueio e soquete do servidor X para o display `:1` conforme sugerido nos avisos:

    ```
    rm /tmp/.X11-unix/X1
    rm /tmp/.X1-lock
    ```

    2.4 Após remover esses arquivos, tente iniciar novamente o servidor `VNC`: `vncserver -geometry 1440x720`

    Isso deve permitir que você inicie um novo servidor `VNC` sem problemas relacionados ao display `:1` estar em uso.

3. **Na distribuição `Linux` via `VNC`:** Instale o `PulseAudio` (se ainda não estiver instalado): Você precisará de permissões de `root` para instalar pacotes. Aqui, você usaria `sudo`, assumindo que você tem permissões de `root` e que o `sudo` está configurado corretamente: `apt install pulseaudio -y`

31. **Edite o arquivo de configuração do PulseAudio:** Configure o `PulseAudio` na sua distribuição `Linux` para enviar áudio para o servidor `PulseAudio` que está sendo executado no `Termux`. Para isso, você precisa definir a variável de ambiente `PULSE_SERVER` para apontar para o endereço `IP` do servidor `PulseAudio` (no caso, o endereço `IP` do seu dispositivo `Android`, que pode ser `127.0.0.1` se estiver tudo no mesmo dispositivo): `export PULSE_SERVER=127.0.0.1`

    Você pode colocar essa linha em seu arquivo `.profile` ou `.bashrc` para que seja definida automaticamente em cada sessão.

32. **Teste o Áudio:** Você pode testar se o áudio está funcionando executando um player de áudio ou um comando de teste de áudio. Reinicie o servidor `VNC` e a sessão `Linux`.


## 3. Conexão a partir de um PC (opcional):

1. Baixe e instale um cliente `VNC Viewer` em seu `PC`.

2. No `Termux` no `Android`, inicie uma sessão com o comando:  `vncserver -geometry 1440x720 -localhost no`

2. Na barra de pesquisa, digite o número do `IP` seguido de dois pontos `:` e da porta: `192.168.0.82:5901`

3. Você poderá ser solicitado pela senha.

Com esses passos você conseguirá controlar o `Linux Ubuntu` a partir do seu computador.

## Referências

[1] USER: VEGADATA. ***Usando seu android como se fosse um pc (sem root, usando o termux com o proot-distro).*** Disponível em: <https://www.youtube.com/watch?v=X7wGQEwJAUk&t=4s>. Youtube. Acessado em: 27/03/2024 17:09.

[2] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). Acessado em: 27/03/2024 17:10.

[3] OPENAI. ***Linux no Android com Termux.*** Disponível em: <https://chat.openai.com/c/d223e7ac-5aa3-4907-a8a3-2edee5022d17> (texto adaptado). Acessado em: 27/03/2023 17:11.

