# warthog_navigation

Crie um workspace e clone os pacotes necessários:
``` bash
$ mkdir -p ~/warthog_ws/src 
$ cd ~/warthog_ws/src
$ git clone https://github.com/warthog-cpr/warthog
$ git clone https://github.com/warthog-cpr/warthog_simulator
$ git clone https://github.com/warthog-cpr/warthog_desktop
$ git clone https://github.com/felipe18mohr/warthog_navigation
```

Instale as dependências e compile:
``` bash
Atualizar o package.xml
$ cd ../ && catkin_make
```

No aquivo **warthog.urdf.xacro** do pacote *warthog_description*, adicione as seguintes linhas:
```xml
  <xacro:include filename="$(find velodyne_description)/urdf/VLP-16.urdf.xacro" />
  <xacro:VLP-16 max_range="40" samples="600" hz="5" lasers="8">
    <origin xyz="0.15 0.0 0.7" rpy="0.0 0.0 0.0"/>
  </xacro:VLP-16>
```

Altere as configurações odom0 e imu0 do arquivo **control.yaml** do pacote *warthog_control* para os seguintes:
```yaml
  odom0_config: [false, false, false,
                 false, false, false,
                 true, true, true,
                 false, false, true,
                 false, false, false]
            
  imu0_config: [false, false, false,
                true, true, true,
                false, false, false,
                true, true, true,
                false, false, false]
```

Launch:
``` bash
$ source devel/setup.bash
$ roslaunch warthog_navigation main.launch
```
