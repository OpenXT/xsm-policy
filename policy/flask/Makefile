#
# Copyright (c) 2010 Citrix Systems, Inc.
# 
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#

# flask needs to know where to export the libselinux headers.
LIBSEL ?= ../../libselinux

# flask needs to know where to export the kernel headers.
LINUXDIR ?= ../../../linux-2.6

AWK = awk

CONFIG_SHELL := $(shell if [ -x "$$BASH" ]; then echo $$BASH; \
          else if [ -x /bin/bash ]; then echo /bin/bash; \
          else echo sh; fi ; fi)

FLASK_H_DEPEND = security_classes initial_sids
AV_H_DEPEND = access_vectors

FLASK_H_FILES = class_to_string.h flask.h initial_sid_to_string.h
AV_H_FILES = av_inherit.h common_perm_to_string.h av_perm_to_string.h av_permissions.h
ALL_H_FILES = $(FLASK_H_FILES) $(AV_H_FILES)

all:  $(ALL_H_FILES)

$(FLASK_H_FILES): $(FLASK_H_DEPEND)
	$(CONFIG_SHELL) mkflask.sh $(AWK) $(FLASK_H_DEPEND)

$(AV_H_FILES): $(AV_H_DEPEND)
	$(CONFIG_SHELL) mkaccess_vector.sh $(AWK) $(AV_H_DEPEND)

tolib: all
	install -m 644 flask.h av_permissions.h $(LIBSEL)/include/selinux
	install -m 644 class_to_string.h av_inherit.h common_perm_to_string.h av_perm_to_string.h $(LIBSEL)/src

tokern: all
	install -m 644 $(ALL_H_FILES) $(LINUXDIR)/security/selinux/include

install: all

relabel:

clean:  
	rm -f $(FLASK_H_FILES)
	rm -f $(AV_H_FILES)
