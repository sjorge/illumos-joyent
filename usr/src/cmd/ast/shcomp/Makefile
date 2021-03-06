#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#

#
# Copyright 2009 Sun Microsystems, Inc.  All rights reserved.
# Use is subject to license terms.
#
# Copyright (c) 2018, Joyent, Inc.
# Copyright 2021 OmniOS Community Edition (OmniOSce) Association.

SHELL= /usr/bin/ksh93

include ../../Makefile.cmd
$(SPARC_BLD)include ../../Makefile.cmd.64

.KEEP_STATE:

# Set common AST build flags (e.g., needed to support the math stuff).
include ../Makefile.ast

OBJECTS= shcomp.o

ASTSRC= $(C_AST)/src/cmd/ksh93
LIBSHELLMACH= $(MACH)
LIBSHELLBASE= ../libshell
LIBSHELLSRC= $(ASTSRC)/sh

SRCS=	$(OBJECTS:%.o=$(LIBSHELLSRC)/%.c)

LDLIBS += -lshell -last

# We use "=" here since using $(CPPFLAGS.master) is very tricky in our
# case - it MUST come as the last element but future changes in -D options
# may then cause silent breakage in the AST sources because the last -D
# option specified overrides previous -D options so we prefer the current
# way to explicitly list each single flag.
CPPFLAGS= \
	$(DTEXTDOM) $(DTS_ERRNO) \
	$(LIBSHELLCPPFLAGS)

CFLAGS += $(ASTCFLAGS)
CFLAGS64 += $(ASTCFLAGS64)

CERRWARN +=	-_gcc=-Wno-parentheses

SMOFF +=	all_func_returns

ROOTCMDDIR=$(ROOT)/usr/bin

PROG= shcomp

%.o:    $(LIBSHELLSRC)/%.c
	$(COMPILE.c) -c -o $@ $<
	$(POST_PROCESS_O)

all: $(PROG)

# dummy file since AST/ksh/shcomp doesn't use *.po files
# (and "shcomp" is just a frontend which calls directly into libshell,
# i.e. there are no l10n strings here)
$(PROG).po:
	$(RM) $(PROG).po ; \
	$(TOUCH) $(PROG).po

install: all $(ROOTCMD)
install_h _feature:

$(PROG):	$(OBJECTS)
	$(RM) shcomp
	$(LINK.c) $(OBJECTS) -o $@ $(LDLIBS)
	$(POST_PROCESS)

clean:
	$(RM) $(OBJECTS)

include ../../Makefile.targ
