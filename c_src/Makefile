
include ../config/include.mk


## don't build this under win32 at all
ifdef WIN32
PRIV_FILES = 
else
PRIV_FILES=../priv/fdsend_drv.so \
	   ../priv/fdsrv \
	   ../priv/fdsrv_drv.so 
endif


CFLAGS += -I$(ERL_C_INCLUDE_DIR) -I../config -I.
LIBS = -lresolv $(SOCKET_LIBS) $(NSL_LIBS)

#
# Targets
#

all: $(PRIV_FILES)

clean:
	-rm -f $(PRIV_FILES) fdsrv_drv.o fdlib_pic.o fdlib.o \
	       fdsend_drv.o fdsrv.o

../priv/fdsrv:	fdsrv.o	fdlib.o
	$(CC) -o $@ fdsrv.o fdlib.o $(LIBS)
	strip $@
	@echo "****** ------------------------------------------------"
	@echo "****** Trying to set the superuser bit for: " $@
	@echo "****** Make sure to setup sudo to run without passwords"
	@echo "****** ------------------------------------------------"
	-sudo chown -v root $@
	-sudo chmod -v u+s $@

../priv/fdsrv_drv.so: fdsrv_drv.o fdlib_pic.o
	$(LD_SHARED) -o $@ fdsrv_drv.o fdlib_pic.o $(LIBS)

../priv/fdsend_drv.so: fdsend_drv.o fdlib_pic.o
	$(LD_SHARED) -o $@ fdsend_drv.o fdlib_pic.o $(LIBS)

fdsrv_drv.o: fdsrv_drv.c
	$(CC) -o $@ -c -fpic $(CFLAGS) -DDYNAMIC_DRIVER fdsrv_drv.c

fdsend_drv.o: fdsend_drv.c
	$(CC) -o $@ -c -fpic $(CFLAGS) -DDYNAMIC_DRIVER fdsend_drv.c

fdlib_pic.o: fdlib.c fdlib.h
	$(CC) -o $@ -c -fpic $(CFLAGS) -DDYNAMIC_DRIVER fdlib.c

