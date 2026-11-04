# 2. Установка

Рекомендуется собирать актуальные версии из исходных кодов. Для macOS используется Homebrew для установки зависимостей, для Linux — системный пакетный менеджер и сборка из исходников.

## 2.1. Общие зависимости

Для сборки SExtractor и SCAMP требуются библиотеки CFITSIO, FFTW, OpenBLAS (или ATLAS) и PLplot. На Linux они устанавливаются через пакетный менеджер, на macOS — через Homebrew.

## 2.2. Linux (Debian / Ubuntu)

Установка зависимостей:

```bash
sudo apt update
sudo apt install -y build-essential autoconf automake libtool pkg-config \
    libcfitsio-dev libfftw3-dev libatlas-base-dev libplplot-dev \
    libcurl4-openssl-dev
```

Сборка SExtractor:

```bash
git clone https://github.com/astromatic/sextractor.git
cd sextractor
./autogen.sh
./configure --prefix=/usr/local
make -j"$(nproc)"
sudo make install
```

Сборка SCAMP:

```bash
git clone https://github.com/astromatic/scamp.git
cd scamp
./autogen.sh
./configure --prefix=/usr/local
make -j"$(nproc)"
sudo make install
```

Проверка: `sex --version`, `scamp -v`.

> 💡 **Примечание.** Для установки в домашний каталог без прав суперпользователя используйте `--prefix=$HOME/.local`. Убедитесь, что `$HOME/.local/bin` присутствует в `$PATH`.

## 2.3. macOS

### 2.3.1. Установка Homebrew

Homebrew — стандартный менеджер пакетов для macOS. Перед установкой необходимы Xcode Command Line Tools; скрипт установки Homebrew обычно предлагает их установить.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

После установки проверьте: `brew --version`. Для Apple Silicon (M1/M2/M3) Homebrew размещается в `/opt/homebrew`; для Intel — в `/usr/local`. Убедитесь, что соответствующий путь добавлен в `$PATH` (команда `brew shellenv` выводит необходимые настройки).

### 2.3.2. Установка зависимостей через Homebrew

```bash
brew install autoconf automake libtool cfitsio fftw openblas plplot
```

После установки зависимостей SExtractor и SCAMP собираются из исходников так же, как в разделе для Linux. Для Apple Silicon при необходимости явно передайте пути к Homebrew:

```bash
git clone https://github.com/astromatic/sextractor.git
cd sextractor
./autogen.sh
CPPFLAGS="-I/opt/homebrew/include" \
LDFLAGS="-L/opt/homebrew/lib" \
./configure --prefix=/usr/local
make -j"$(sysctl -n hw.ncpu)"
sudo make install
```

```bash
git clone https://github.com/astromatic/scamp.git
cd scamp
./autogen.sh
CPPFLAGS="-I/opt/homebrew/include" \
LDFLAGS="-L/opt/homebrew/lib" \
./configure --prefix=/usr/local
make -j"$(sysctl -n hw.ncpu)"
sudo make install
```

> ⚠️ **Внимание (Apple Silicon).** Для Intel-маков пути заменяются на `/usr/local/include` и `/usr/local/lib`. Если `configure` не находит `cfitsio`, проверьте, что флаги `CPPFLAGS` и `LDFLAGS` указывают на каталог Homebrew, соответствующий архитектуре.

## 2.4. Windows

Рекомендуется использовать WSL (Windows Subsystem for Linux) с дистрибутивом Ubuntu. Процесс установки соответствует разделу 2.2.

```bash
sudo apt update
sudo apt install -y build-essential autoconf automake libtool pkg-config \
    libcfitsio-dev libfftw3-dev libatlas-base-dev libplplot-dev

git clone https://github.com/astromatic/sextractor.git
cd sextractor && ./autogen.sh && ./configure --prefix=/usr/local \
    && make -j"$(nproc)" && sudo make install

git clone https://github.com/astromatic/scamp.git
cd scamp && ./autogen.sh && ./configure --prefix=/usr/local \
    && make -j"$(nproc)" && sudo make install
```
