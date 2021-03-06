# dockerfile
FROM openjdk:8-jre-slim
MAINTAINER Kamil Cukrowski <kamilcukrowski@gmail.com>

ENV SW4STM32_VERSION 2.3

# eclipse needs java8
# make is needed by some projects with makefile
# bzip2 needed to unpack fr.ac6.mcu.externaltools.arm-none and gets uninstalled later
# actually bzip2 and unzip and zip may be needed 
RUN set -ex ; \
	echo '===> Install prerequisites'; \
	apt-get update ; \
	apt-get install -y --no-install-recommends openjdk-8-jre-headless make curl bzip2 sudo ; \
	\
	echo '===> Downloading sw4stm32' ; \
	curl -O http://www.ac6-tools.com/downloads/SW4STM32/install_sw4stm32_linux_64bits-v${SW4STM32_VERSION}.run ; \
	\
	echo '===> Check md5sum' ; \
	curl    http://www.ac6-tools.com/downloads/SW4STM32/install_sw4stm32_linux_64bits-v${SW4STM32_VERSION}.run.md5\
		| md5sum -c - ; \
	\
	echo '===> Installing sw4stm32' ; \
	mkdir -p /etc/udev/rules.d ; \
	yes | java -DINSTALL_PATH=/opt/sw4stm32 -d64 \
		-jar install_sw4stm32_linux_64bits-v${SW4STM32_VERSION}.run -options-system ; \
	rm -v install_sw4stm32_linux_64bits-v${SW4STM32_VERSION}.run ; \
	\
	echo '===> Create sw4stm32 shortcut' ; \
	printf '#!/bin/sh\nexec /opt/sw4stm32/eclipse "$@"\n' > /usr/local/bin/sw4stm32 ; \
	chmod +x /usr/local/bin/sw4stm32 ; \
	echo '===> Create sw4stm32-headless-build.sh shortcut' ; \
	printf '#!/bin/sh\nexec java -jar /opt/sw4stm32/plugins/org.eclipse.equinox.launcher_*.jar -nosplash -application org.eclipse.cdt.managedbuilder.core.headlessbuild "$@"\n' > /usr/local/bin/sw4stm32-headless-build.sh ; \
	chmod +x /usr/local/bin/sw4stm32-headless-build.sh ; \
	\
	echo '===> Unpacking fr.ac6.mcu.externaltools.arm-none plugin' ; \
	echo '===> ^ ^ Why sw4stm32 is too stupid to do this on its own ?!' ; \
	cd /opt/sw4stm32/plugins/fr.ac6.mcu.externaltools.arm-none.linux64_*/tools ; \
	mkdir -p compiler ; cd ./compiler && \
	tar xjvf ../st-gnu-arm-gcc-*.tar.bz2 ; \
	mv st-gnu-arm-gcc-*/* . ; \
	rmdir st-gnu-arm-gcc-* ; \
	echo '===> Finished unpacking arm-none-eabi-* for sw4stm32' ; \
	\
	echo '===> Cleanup' ; \
	SUDO_FORCE_REMOVE=yes apt-get remove --purge -y bzip2 sudo curl ; \
	apt-get --purge -y autoremove ; \
	rm -rf /var/lib/apt/lists/ ; \
	\
	echo '===> DONE'

# this setup is enough for batch/headless run of sw4stm32


