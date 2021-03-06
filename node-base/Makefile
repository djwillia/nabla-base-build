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
Dockerfile \
Makefile \

.PHONY: rumprun-packages-nodejs
rumprun-packages-nodejs: rumprun solo5 FORCE
	install -m 664 -D $(RUMPPKGS)/config.mk.dist $(RUMPPKGS)/config.mk
	source $(RUMPRUN)/obj/config-PATH.sh && make -C $(RUMPPKGS)/nodejs all node.seccomp
	install -m 775 -D $(RUMPPKGS)/nodejs/node.seccomp node.nabla

rumprun-packages-nodejs-clean:
	make -C $(RUMPPKGS)/nodejs clean

# XXX: This is a temporary horrible hack which should disappear as soon as we
# fix a rumprun issue we introduced by trying to mount the root which is over
# writing rumprun's /dev devices (like random).
dev:
	mkdir -p dev
	dd if=/dev/urandom of=dev/urandom count=1024 bs=1024
	dd if=/dev/urandom of=dev/random count=1024 bs=1024

build_docker: submodule_warning $(FILES) dev rumprun-packages-nodejs
	sudo docker build -f Dockerfile -t nabla-node-base .
	sudo docker tag nabla-node-base nablact/nabla-node-base

clean:
	rm -rf dev node.nabla

distclean: clean rumprun-packages-nodejs-clean solo5-clean rumprun-clean
