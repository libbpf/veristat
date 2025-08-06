# SPDX-License-Identifier: BSD-2-Clause
VERISTAT_VERSION ?= 0.5.1
DEBUG ?=

OUTPUT := .output
LIBBPF_SRC := $(abspath ../libbpf/src)
LIBBPF_OBJ := $(abspath $(OUTPUT)/libbpf.a)
# Use our own libbpf API headers and Linux UAPI headers distributed with
# libbpf to avoid dependency on system-wide headers, which could be missing or
# outdated
INCLUDES := -I$(OUTPUT) -I../libbpf/include/uapi
CFLAGS := -g -Wall -Wno-format-truncation -DVERISTAT_VERSION=\"$(VERISTAT_VERSION)\" -O$(if $(DEBUG),0,2)
ALL_CFLAGS := $(CFLAGS) $(EXTRA_CFLAGS)
ALL_LDFLAGS := $(LDFLAGS) $(EXTRA_LDFLAGS)

export VERISTAT_VERSION

ifeq ($(V),1)
	Q =
	msg =
else
	Q = @
	msg = @printf '  %-8s %s%s\n'					\
		      "$(1)"						\
		      "$(patsubst $(abspath $(OUTPUT))/%,%,$(2))"	\
		      "$(if $(3), $(3))";
	MAKEFLAGS += --no-print-directory
endif

define allow-override
  $(if $(or $(findstring environment,$(origin $(1))),\
            $(findstring command line,$(origin $(1)))),,\
    $(eval $(1) = $(2)))
endef

$(call allow-override,CC,$(CROSS_COMPILE)cc)
$(call allow-override,LD,$(CROSS_COMPILE)ld)

# for installation
INSTALL := install
prefix := /usr/local
bindir := $(prefix)/bin

.PHONY: all
all: veristat

.PHONY: clean
clean:
	$(call msg,CLEAN)
	$(Q)rm -rf $(OUTPUT) veristat

.PHONY: install
install: all
	mkdir -p $(DESTDIR)$(bindir)/
	$(INSTALL) -pt $(DESTDIR)$(bindir)/ veristat

$(OUTPUT) $(OUTPUT)/libbpf:
	$(call msg,MKDIR,$@)
	$(Q)mkdir -p $@

# Build libbpf
$(LIBBPF_OBJ): $(wildcard $(LIBBPF_SRC)/*.[ch] $(LIBBPF_SRC)/Makefile) | $(OUTPUT)/libbpf
	$(call msg,LIB,$@)
	$(Q)$(MAKE) -C $(LIBBPF_SRC) BUILD_STATIC_ONLY=1		      \
		    OBJDIR=$(dir $@)/libbpf DESTDIR=$(dir $@)		      \
		    INCLUDEDIR= LIBDIR= UAPIDIR=			      \
		    EXTRA_CFLAGS="-g -O$(if $(DEBUG),0,2) $(EXTRA_CFLAGS)"    \
		    install

# Build .c files
$(OUTPUT)/%.o: %.c $(wildcard %.h) $(LIBBPF_OBJ) | $(OUTPUT)
	$(call msg,CC,$@)
	$(Q)$(CC) $(ALL_CFLAGS) $(INCLUDES) -c $(filter %.c,$^) -o $@

# Build application binary
veristat: $(OUTPUT)/veristat.o $(LIBBPF_OBJ) | $(OUTPUT)
	$(call msg,BINARY,$@)
	$(Q)$(CC) $(ALL_CFLAGS) $^ $(ALL_LDFLAGS) -lelf -lz -o $@

# delete failed targets
.DELETE_ON_ERROR:

# keep intermediate (.skel.h, .bpf.o, etc) targets
.SECONDARY:
