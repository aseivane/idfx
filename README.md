# idfx :zap:

While there is [no support for USB devices on WSL2](https://github.com/microsoft/WSL/issues/4322) for now, this tool comes to help you to flash and monitor [ESP-IDF](https://github.com/espressif/esp-idf) and [ESP8266_SDK](https://github.com/espressif/ESP8266_RTOS_SDK) applications on the [WSL2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions).

> **Info:**<br>Tested on [Ubuntu 20.04 LTS](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71) and [Debian](https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6) distributions.

> **Note:**<br>As a prerequisite for using this tool, [Python :snake:](https://www.python.org) needs to be installed on the Windows.

# Supported ESP-IDF versions

`idfx` supports:
- [ESP-IDF](https://github.com/espressif/esp-idf) version 4.0 and above
- [ESP8266_SDK](https://github.com/espressif/ESP8266_RTOS_SDK) version 3.0 and above


## Intro

[WSL2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions) still [does not support USB devices](https://github.com/microsoft/WSL/issues/4322), but with a little effort we can make possible to flash and monitor ESP device from WSL2.

> **Info:**<br>Tested on [Ubuntu 20.04 LTS](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71)  and [Debian](https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6) distributions.

For flashing and monitoring over the serial COM port, I've wrote this compact [idfx](https://github.com/abobija/idfx) shell script.

> **Note:**<br>As a prerequisite for using `idfx`, [Python :snake:](https://www.python.org) needs to be installed on the Windows.

More about `idfx` you can find in [official repository](https://github.com/abobija/idfx).

# Installation :rocket:

- Update Linux<br>
`sudo apt update && sudo apt upgrade -y`

- Install tools required for esp-idf<br>
`sudo apt install -y git wget curl flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0`

- Make python3 as default python<br>
`sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1`

- Make pip3 as default pip<br>
`sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1`

- Make esp directory and go inside<br>
`cd ~ && mkdir esp && cd esp`

- Clone esp-idf repository<br>
`git clone --recursive https://github.com/espressif/esp-idf.git`

- Modify esp-idf installation script to work with WSL<br>
`cd esp-idf && ! grep -q "dirname --" install.sh; [ $? -eq 0 ] && sed -i 's/dirname/dirname --/g' install.sh`

- Run esp-idf installation script<br>
`. ./install.sh || true`

- Add esp-idf export script to the user profile script in order to make isp-idf tools visible on the PATH for every session<br>
`echo -e '\n\n. $HOME/esp/esp-idf/export.sh > /dev/null 2>&1 || true' >> ~/.profile`

- Install **idfx** ( [**?**](https://github.com/abobija/idfx) )<br>
`curl https://git.io/JyBgj --create-dirs -L -o $HOME/bin/idfx && chmod u+x $HOME/bin/idfx`

- Source profile script to add all necessary tools to the PATH<br>
`. ~/.profile || true`

### Create (a copy of hello_world) project :page_facing_up:

- Make a copy of hello_world example project in home directory<br>
`cd ~ && cp -r $IDF_PATH/examples/get-started/hello_world .`

# Usage

Signature:

```
idfx COMMAND [PORT]
```

Where the `PORT` is serial COM Port on the Windows (use the Device Manager to find your port).

For the full usage please execute next command:

```
idfx help
```

### Build, flash and monitor :zap:

- Go inside of project<br>
`cd hello_world`

- Set target<br>
`idf.py set-target esp32`

- Configure<br>
`idf.py menuconfig`

- Build<br>
`idf.py build`

- Flash<br>
<sup>(open Device Manager on Windows to find COM port of your ESP, mine is `COM2`)</sup><br>
`idfx flash COM2`

- Monitor<br>
<sup>(change `COM2` with your port)</sup><br>
`idfx monitor COM2`<br>
<sub>(To exit monitor press `CTRL+]` or `CTRL+T`,`X`)</sub>

> **Tip:**<br>Flash and monitor with single command:<br>`idfx flash COM2 monitor`

<br>
That's it.<br>
Happy coding and flashing! :zap:
<br>
<br>

![idfx preview](https://user-images.githubusercontent.com/45392201/99966731-090b9e00-2d97-11eb-947b-c7b4f9d8fe25.gif)


# Examples

For most of the cases (when you edit the code of your application) you can use `idfx all COM2` because this command will build, flash and monitor your app, at once. Of course, you need to change `COM2` (in previous command) with correct COM port.

| Command  | Description |
| ------------- | ------------- |
| `idfx all COM2` | Build project, flash and monitor serial output, using port `COM2` |
| `idfx build`  | Build project |
| `idfx flash COM2`  | Flashing project using port `COM2` |
| `idfx monitor COM2`  | Display serial output of port `COM2` |
| `idfx flash COM2 monitor` | Flash project and display serial output, using port `COM2` |
| `idfx erase-flash COM2` | Erase the entire flash, using port `COM2` |
| `idfx help` | Show the `idfx` usage |

# Author

[abobija](https://github.com/abobija) - [abobija.com](https://abobija.com)

# License

[MIT](LICENSE)
