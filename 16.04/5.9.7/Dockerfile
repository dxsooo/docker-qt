FROM ubuntu:16.04

ENV QT_VERSION 5.9.7

# base
RUN apt-get -qq update && \
    apt-get install -qqy build-essential perl python python-pip git cmake wget && \
    apt-get install -qqy '^libxcb.*-dev' libx11-xcb-dev libglu1-mesa-dev libxrender-dev libxi-dev libxt-dev && \
    apt-get install -qqy libssl1.0-dev libxcursor-dev libxcomposite-dev libxdamage-dev libxrandr-dev libdbus-1-dev libfontconfig1-dev libcap-dev libxtst-dev libpulse-dev libudev-dev libpci-dev libnss3-dev libasound2-dev libxss-dev libegl1-mesa-dev gperf bison && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# clone & build Qt
RUN git clone https://code.qt.io/qt/qt5.git && cd qt5 && git checkout v$QT_VERSION && \
    ./init-repository --module-subset=default,-qtwebengine,-qtcharts,-qtdatavis3d,-qtpurchasing,-qtvirtualkeyboard,-qtnetworkauth,-qtremoteobjects,-qtscript,-qtspeech,-qtandroidextras,-qtmacextras,-qtwinextras,-qtactiveqt && \
    mkdir qt5-build && cd qt5-build && \
    ../configure -opensource -nomake examples -nomake tests -confirm-license && \
    make -j8 && make install && \
    cd ../.. && rm -rf qt5

# deployment tool
ENV QTDIR /usr/local/Qt-$QT_VERSION
RUN wget -nv https://github.com/probonopd/linuxdeployqt/releases/download/5/linuxdeployqt-5-x86_64.AppImage && \
    chmod a+x linuxdeployqt-5-x86_64.AppImage && \
    ./linuxdeployqt-5-x86_64.AppImage --appimage-extract && \
    mv squashfs-root $QTDIR/bin/linuxdeployqt-extracted && \
    ln -s $QTDIR/bin/linuxdeployqt-extracted/AppRun $QTDIR/bin/linuxdeployqt-x86_64.AppImage && \
    rm linuxdeployqt-5-x86_64.AppImage

# others
