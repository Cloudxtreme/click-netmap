# For multiple programs using a single source file each,
# we can just define 'progs' and create custom targets.
PROGS	=	pkt-gen bridge vale-ctl
#PROGS += pingd
PROGS	+= testlock test_select testmmap vale-ctl
LIBNETMAP =

CLEANFILES = $(PROGS) *.o
NO_MAN=
CFLAGS = -O2 -pipe
CFLAGS += -Werror -Wall -Wunused-function
CFLAGS += -I ../sys # -I/home/luigi/FreeBSD/head/sys -I../sys
CFLAGS += -Wextra
ifdef WITH_PCAP
# do not use pcap by default, as it is not always available on linux
LDLIBS += -lpcap
else
CFLAGS += -DNO_PCAP
endif

LDLIBS += -lpthread
ifeq ($(shell uname),Linux)
	LDLIBS += -lrt	# on linux
endif
#SRCS = pkt-gen.c

all: $(PROGS)

kern_test: testmod/kern_test.c
	$(CC) $(LDLIBS) $(INSTALL_NETMAP_APPS)/$^ -o $(INSTALL_NETMAP_APPS)/$@ 

pkt-gen: pkt-gen.o
	$(CC) $(LDLIBS) $(INSTALL_NETMAP_APPS)/$^ -o $(INSTALL_NETMAP_APPS)/$@ 

bridge: bridge.o
	$(CC) $(LDLIBS) $(INSTALL_NETMAP_APPS)/$^ -o $(INSTALL_NETMAP_APPS)/$@ 

vale-ctl: vale-ctl.o
	$(CC) $(LDLIBS) $(INSTALL_NETMAP_APPS)/$^ -o $(INSTALL_NETMAP_APPS)/$@ 

%-pic.o: %.c
	$(CC) $(CFLAGS) -fpic -c $^ -o $(INSTALL_NETMAP_APPS)/$@

%.o: %.c
	$(CC) $(CFLAGS) -c $^ -o $(INSTALL_NETMAP_APPS)/$@

clean:
	-@rm -rf $(CLEANFILES)

testlock: testlock.c
	$(CC) $(CFLAGS) -fpic -c $^ -o $(INSTALL_NETMAP_APPS)/$@
test_select: test_select.c
	$(CC) $(CFLAGS) -fpic -c $^ -o $(INSTALL_NETMAP_APPS)/$@
testmmap: testmmap.c
	$(CC) $(CFLAGS) -fpic -c $^ -o $(INSTALL_NETMAP_APPS)/$@
