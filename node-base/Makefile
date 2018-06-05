# Copyright (c) 2018, IBM
# Author(s): Brandon Lum, Ricardo Koller
#
# Permission to use, copy, modify, and/or distribute this software for
# any purpose with or without fee is hereby granted, provided that the
# above copyright notice and this permission notice appear in all
# copies.
#
# THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL
# WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE
# AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL
# DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA
# OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
# TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
# PERFORMANCE OF THIS SOFTWARE.

build: build_docker.alpine

../build/node.nablet: FORCE
	make -C ../ build/node.nablet

../build/ukvm-bin: FORCE
	make -C ../ build/ukvm-bin

../build/nabla_run: FORCE
	make -C ../ build/nabla_run

node.nablet: ../build/node.nablet
	install ../build/node.nablet .

ukvm-bin: ../build/ukvm-bin
	install ../build/ukvm-bin .

nabla_run: ../build/nabla_run
	install ../build/nabla_run .

FORCE:

build_docker.alpine: Dockerfile.alpine nabla_run node.nablet ukvm-bin
	sudo docker build -f Dockerfile.alpine -t nabla-node-base:alpine .
	sudo docker tag nabla-node-base:alpine kollerr/nabla-node-base:alpine

# Don't pass a mountpoint, it runs a node app that just prints 'Hello, Rump!!'
# It doesn't create an ISO either.
run_docker.alpine:
	sudo docker run --rm -it --cap-add=NET_ADMIN nabla-node-base:alpine

clean:
	rm -f nabla_run node.nablet ukvm-bin