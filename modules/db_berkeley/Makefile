# $Id$
#
# example module makefile
#
# 
# WARNING: do not run this directly, it should be run by the master Makefile

# extra debug messages
# -DBDB_EXTRA_DEBUG is optional


include ../../Makefile.defs 
auto_gen=
NAME=db_berkeley.so

SCRIPTS_DIR?=../../scripts/
UTILS_DIR?=../../utils/

DEFS +=-I$(LOCALBASE)/include -I$(LOCALBASE)/BerkeleyDB.4.6/include \
	-I$(SYSBASE)/include -I$(LOCALBASE)/include/db46 

IS_BSD= $(shell echo "$(OS)" | sed -e 's/^.*bsd/yes/i' )
ifeq ($(IS_BSD), yes)
	DB_LIB=ldb-4.6
else
	DB_LIB=ldb
endif

LIBS=-L$(LOCALBASE)/lib -L$(SYSBASE)/lib -L$(LOCALBASE)/BerkeleyDB.4.6/lib \
		-$(DB_LIB)

include ../../Makefile.modules

.PHONY: $(UTILS_DIR)/db_berkeley/bdb_recover
$(UTILS_DIR)/db_berkeley/bdb_recover:
	make -C $(UTILS_DIR)/db_berkeley/

install_module_custom: $(bin-prefix)/$(bin-dir) $(UTILS_DIR)/db_berkeley/bdb_recover
	mkdir -p $(modules-prefix)/$(lib-dir)/opensipsctl ; \
	sed -e "s#/usr/local/share/opensips/#$(data-target)#g" \
		< $(SCRIPTS_DIR)/opensipsctl.db_berkeley > /tmp/opensipsctl.db_berkeley ; \
	$(INSTALL_CFG) /tmp/opensipsctl.db_berkeley \
		$(modules-prefix)/$(lib-dir)/opensipsctl/opensipsctl.db_berkeley ; \
	rm -fr /tmp/opensipsctl.db_berkeley ; \
	sed -e "s#/usr/local/share/opensips#$(data-target)#g" \
		< $(SCRIPTS_DIR)/opensipsdbctl.db_berkeley > /tmp/opensipsdbctl.db_berkeley ; \
	$(INSTALL_TOUCH) $(modules-prefix)/$(lib-dir)/opensipsctl/opensipsdbctl.db_berkeley ; \
	$(INSTALL_CFG) /tmp/opensipsdbctl.db_berkeley $(modules-prefix)/$(lib-dir)/opensipsctl/ ; \
	rm -fr /tmp/opensipsdbctl.db_berkeley ; \
	mkdir -p $(data-prefix)/$(data-dir)/db_berkeley/opensips ; \
	for FILE in $(wildcard $(SCRIPTS_DIR)/db_berkeley/opensips/*) ; do \
		if [ -f $$FILE ] ; then \
		$(INSTALL_TOUCH) $$FILE \
			$(data-prefix)/$(data-dir)/db_berkeley/opensips/`basename "$$FILE"` ; \
		$(INSTALL_CFG) $$FILE \
			$(data-prefix)/$(data-dir)/db_berkeley/opensips/`basename "$$FILE"` ; \
		fi ;\
	done ; \
	$(INSTALL_BIN) $(UTILS_DIR)/db_berkeley/bdb_recover $(bin-prefix)/$(bin-dir) ; \
