# Copyright (c) 2018 Contributors as noted in the AUTHORS file
#
# Permission to use, copy, modify, and/or distribute this software
# for any purpose with or without fee is hereby granted, provided
# that the above copyright notice and this permission notice appear
# in all copies.
#
# THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL
# WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE
# AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR
# CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS
# OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT,
# NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN
# CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

export TOP=$(abspath ..)
all: build_docker

RUMPPKGS=$(TOP)/rumprun-packages
RUMPRUN=$(TOP)/rumprun
include ../Makefile.inc

FILES=\
rootfs/etc/services \
rootfs/etc/pwd.db \
rootfs/etc/group \
rootfs/etc/nsswitch.conf \
rootfs/etc/spwd.db \
rootfs/etc/hosts \
rootfs/etc/master.passwd \
rootfs/etc/protocols \
rootfs/etc/resolv.conf \
Dockerfile \
Makefile \

.PHONY: rumprun-packages-python3
rumprun-packages-python3: rumprun solo5 FORCE
	install -m 664 -D $(RUMPPKGS)/config.mk.dist $(RUMPPKGS)/config.mk
	source $(RUMPRUN)/obj/config-PATH.sh && make -C $(RUMPPKGS)/python3 all python.seccomp
	install -m 775 -D $(RUMPPKGS)/python3/python.seccomp python3.nabla

rumprun-packages-python3-clean:
	make -C $(RUMPPKGS)/python3 clean

# XXX: This is a temporary horrible hack which should disappear as soon as we
# fix a rumprun issue we introduced by trying to mount the root which is over
# writing rumprun's /dev devices (like random).
dev:
	mkdir -p dev
	dd if=/dev/urandom of=dev/urandom count=1024 bs=1024
	dd if=/dev/urandom of=dev/random count=1024 bs=1024

build_docker: submodule_warning $(FILES) dev rumprun-packages-python3
	sudo docker build -f Dockerfile -t nabla-python3-base .
	sudo docker tag nabla-python3-base nablact/nabla-python3-base

clean:
	rm -rf dev python3.nabla

distclean: clean rumprun-packages-python3-clean solo5-clean rumprun-clean
