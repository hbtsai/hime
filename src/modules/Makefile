include ../../config.mak
include ../suffixes-rule

CFLAGS+=-I.. -I../IMdkit/include
HIME_MODULE=intcode-module.so

ifeq ($(USE_ANTHY),Y)
HIME_MODULE+=anthy-module.so
CFLAGS += $(ANTHY_INC)
endif

ifeq ($(USE_CHEWING),Y)
HIME_MODULE+=chewing-module.so
CHEWING_DATADIR=$(shell pkg-config --variable=datadir chewing)
CFLAGS += -DCHEWING_DATADIR=\"$(CHEWING_DATADIR)\" $(shell pkg-config --cflags chewing)
endif

.PHONY: all
all:	$(HIME_MODULE)

anthy_module_so = anthy.pico
anthy-module.so: $(anthy_module_so)
	@echo "linking $@ ..."
	$(CCLD) $(SO_FLAGS) -o $@ $(anthy_module_so) $(LDFLAGS) $(ANTHY_INC)

intcode_module_so = intcode.pico win-int.pico
intcode-module.so: $(intcode_module_so)
	@echo "linking $@ ..."
	$(CCLD) $(SO_FLAGS) -o $@ $(intcode_module_so) $(LDFLAGS)

chewing_module_obj = chewing-conf.pico
chewing_module_so = chewing.pico hime-setup-chewing.pico
chewing-module.so: $(chewing_module_obj) $(chewing_module_so)
	@echo "linking $@ ..."
	$(CCLD) $(SO_FLAGS) -o $@ $(chewing_module_obj) $(chewing_module_so) $(LDFLAGS) $(shell pkg-config --cflags --libs chewing)

.PHONY: install
install:
	install $(HIME_MODULE) "$(himelibdir)"

.PHONY: unintall
uninstall:
	cd $(himelibdir) && rm -f $(HIME_MODULE)

.PHONY: clean
clean:
	@echo "clean up"
	@rm -f *.pico *.so *.o
