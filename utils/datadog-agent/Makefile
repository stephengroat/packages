include $(TOPDIR)/rules.mk

PKG_NAME:=datadog-agent
PKG_RELEASE:=1
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

GO_PKG:=github.com/DataDog/datadog-agent
GO_PKG_BUILD_PKG:= \
  $(GO_PKG)/cmd/proxy \
  $(GO_PKG)/cmd/dnet

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://$(GO_PKG)
PKG_SOURCE_VERSION:=59d7e33dae4fa9d23474397a310389b488329319
PKG_SOURCE_DATE:=2021-07-22
PKG_MIRROR_HASH:=2a3704abb4c5c65b981b83851f46d5a24c4f44f566b76d9067d4bfa78e922e27

PKG_MAINTAINER:=Stephen Groat <stephen@groat.us>

PKG_BUILD_DEPENDS:=golang/host python3/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/datadog-agent
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Datadog agent
  URL:=https://github.com/DataDog/datadog-agent
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/datadog-agent/description
datadog-agent is the agent for the Datadog (https://datadoghq.com) monitoring & security platform
endef

GO_PKG_BUILD_VARS += GO111MODULE=auto

define Package/datadog-agent/build
	$(pip install -r requirements.txt)
	$(invoke install-tools)
	$(invoke deps)
	$(invoke agent.build --build-exclude=systemd)
endef

define Package/datadog-agent/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/dnet $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/proxy $(1)/usr/bin/docker-proxy
endef

$(eval $(call GoBinPackage,datadog-agent))
