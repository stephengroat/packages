include $(TOPDIR)/rules.mk

PKG_NAME:=datadog-agent
PKG_RELEASE:=0
PKG_VERSION:=7.0.29
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

GO_PKG:=github.com/DataDog/datadog-agent
GO_PKG_BUILD_PKG:=$(GO_PKG)/cmd/agent

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://$(GO_PKG).git
PKG_MIRROR_HASH:=56fbc64ab28a0c31d1ec02132d073312257b1b2cf8dc0e3bcf85df6045bd79b1
PKG_SOURCE_VERSION:=59d7e33dae4fa9d23474397a310389b488329319
PKG_SOURCE_DATE:=2021-07-22

PKG_MAINTAINER:=Stephen Groat <stephen@groat.us>

PKG_BUILD_DEPENDS:=golang/host python3/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk
include ../../lang/golang/golang-package.mk
include ../../lang/python/python3-package.mk

define Package/datadog-agent
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Datadog agent
  URL:=https://github.com/DataDog/datadog-agent
  DEPENDS:=$(GO_ARCH_DEPENDS) +python3
endef

CMAKE_OPTIONS+= -DBUILD_DEMO:BOOL=OFF -DCMAKE_INSTALL_PREFIX:PATH=$(PKG_BUILD_DIR)/dev -DDISABLE_PYTHON2:BOOL=ON  -DDISABLE_PYTHON3:BOOL=OFF
CMAKE_SOURCE_SUBDIR=rtloader

define Build/Configure
	$(call Build/Configure/Default)
	$(call GoPackage/Build/Configure)
endef

define Build/Compile
	$(call Build/Compile/Default,rtloader)
	$(call GoPackage/Build/Compile,-tags "kubelet docker secrets kubeapiserver gce apm zk jmx ec2 etcd orchestrator zlib python consul process")
endef

$(eval $(call BuildPackage,datadog-agent))
