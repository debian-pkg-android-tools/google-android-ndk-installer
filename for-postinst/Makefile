PKG_NAME:=android-ndk
PKG_VERSION:=12

PKG_SOURCE:=${PKG_NAME}-r${PKG_VERSION}-linux-x86_64.zip
PKG_SOURCE_URL:=${DL_MIRROR}/android/repository/${PKG_SOURCE}

DL_DIR=/var/cache/google-${PKG_NAME}-installer
UNPACK_DIR=$(DL_DIR)/${PKG_NAME}-r${PKG_VERSION}
INSTALL_DIR=/usr/lib/${PKG_NAME}
DOC_DIR=/usr/share/doc/google-${PKG_NAME}

all: $(UNPACK_DIR)/source.properties

install: all
	install -d -m0755 $(DOC_DIR)
	install -m0644 $(UNPACK_DIR)/source.properties $(DOC_DIR)/
	find ${DOC_DIR} | sort >> /var/lib/dpkg/info/google-${PKG_NAME}-installer.list
	@if [ ! -d $(INSTALL_DIR) ]; then \
		mv $(UNPACK_DIR) $(INSTALL_DIR) ; \
		find ${INSTALL_DIR} | sort >> /var/lib/dpkg/info/google-${PKG_NAME}-installer.list ;\
	else\
		echo "\033[0;31m$(INSTALL_DIR) already exists.\033[0m" ; \
		exit 1 ; \
	fi

$(UNPACK_DIR)/source.properties: $(DL_DIR)/$(PKG_SOURCE)
	cd $(DL_DIR) && unzip -ou $(PKG_SOURCE)

$(DL_DIR)/$(PKG_SOURCE):
	cd $(DL_DIR) && \
		su nobody -s /bin/sh -c "wget --continue $(PKG_SOURCE_URL)"
	sha1sum -c $(PKG_SOURCE).sha1

clean:
	-rm -rf -- $(UNPACK_DIR)

distclean: clean
	-rm -rf -- $(DL_DIR)

.PHONY: install clean
