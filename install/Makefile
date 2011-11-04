# FOSSology Makefile - install/
# Copyright (C) 2008-2011 Hewlett-Packard Development Company, L.P.
TOP=..
VARS=$(TOP)/Makefile.conf
include $(VARS)

CONFPATH=$(SYSCONFDIR)

all:	fo-postinstall defconf/Db.conf defconf/fossology.conf

# include the preprocessing stuff
include $(TOP)/Makefile.process

# generate the postinstall script
fo-postinstall:	fo-postinstall-process
	chmod +x fo-postinstall

# generate the default Db.conf file
defconf/Db.conf: defconf/Db.conf-process

# generate fossology.conf
defconf/fossology.conf:	defconf/fossology.conf-process

install: all
	$(INSTALL_PROGRAM) fo-postinstall $(DESTDIR)$(LIBEXECDIR)/fo-postinstall

	echo "Making configuration directories"; \
	if [ ! -e $(DESTDIR)$(CONFPATH) ] ; then \
		mkdir -p $(DESTDIR)$(CONFPATH); \
	fi
	if [ ! -e $(DESTDIR)$(CONFPATH)/mods-enabled ] ; then \
                mkdir -p $(DESTDIR)$(CONFPATH)/mods-enabled; \
                /bin/chmod 775 $(DESTDIR)$(CONFPATH)/mods-enabled; \
        fi
        
	echo "Installing configuration files..."
	if [ ! -f $(DESTDIR)$(CONFPATH)/Db.conf -o "$(OVERWRITE)" ] ; then \
		echo "NOTE: using default version for $(DESTDIR)$(CONFPATH)/Db.conf"; \
	 	$(INSTALL) -m 640 defconf/Db.conf $(DESTDIR)$(CONFPATH)/Db.conf; \
	else \
		echo "WARNING: $(DESTDIR)$(CONFPATH)/Db.conf already exists."; \
		echo "  Not overwriting, consider checking it by hand or use the OVERWRITE option."; \
	fi \
	
	if [ ! -f $(DESTDIR)$(CONFPATH)/fossology.conf -o "$(OVERWRITE)"]; then \
		echo "NOTE: using default version for $(DESTDIR)$(CONFPATH)/fossology.conf"; \
	 	$(INSTALL) -m 666 defconf/fossology.conf $(DESTDIR)$(CONFPATH)/fossology.conf; \
	else \
		echo "WARNING: $(DESTDIR)$(CONFPATH)/fossology.conf already exists."; \
		echo "  Not overwriting, consider checking it by hand or use the OVERWRITE option."; \
	fi \

uninstall:
	rm -f $(DESTDIR)$(LIBEXECDIR)/fo-postinstall

	@if [ -d $(DESTDIR)$(LIBEXECDIR) ]; then \
		if [ "`ls -A $(DESTDIR)$(LIBEXECDIR)`" ]; then \
			echo "WARNING: $(DESTDIR)$(LIBEXECDIR) not empty, not removing";\
		else \
			rmdir $(DESTDIR)$(LIBEXECDIR); \
		fi; \
	fi

	@echo "WARNING: Cowardly refusing to remove the following"
	@echo "      $(DESTDIR)$(CONFPATH)/agents.d"
	@echo "      $(DESTDIR)$(CONFPATH)/fossology.conf"
	@echo "      $(DESTDIR)$(CONFPATH)/Db.conf"
	@echo "  Remove by hand if you desire."


clean:
	rm -f fo-postinstall defconf/Db.conf

test: all
	@echo "NOTICE: no tests available for common"

coverage:
        @echo "NOTICE: no coverage available for common""

.PHONY: all install uninstall clean test