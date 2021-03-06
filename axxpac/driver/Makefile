# axxPac driver library

LIBNAME   = axxPacDrv
LIBNAMEP  = axxPacDrvProto
CREATOR   = axPC

OBJECTS   = $(LIBNAME).o TrapHook.o fsel.o filesys.o errors.o
OBJECTS3X = smartmedia_3x.o
OBJECTSP  = smartmedia.o

TOOLBASE = m68k-palmos-
CC = $(TOOLBASE)gcc -nostartfiles
LD = m68k-palmos-gcc -nostartfiles
XTRALIBS =
BUILDPRC = build-prc
PILRC = pilrc

CSFLAGS = -O2 -S
#DEBUG = -DDEBUG
CFLAGS = -palmos3.1 -O2 -DCREATOR=\'$(CREATOR)\' $(DEBUG)
CFLAGSOPT = $(CFLAGS) -fomit-frame-pointer
CFLAGS3X = $(CFLAGSOPT) -DPALM3X

all: ../$(LIBNAME).prc # ../$(LIBNAMEP).prc

fsel.o: fsel.c
	$(CC) $(CFLAGSOPT) -o $@ -c $<

driver-sections.o: driver-sections.s
	$(CC) -o $@ -c $<

errors.o: errors.c
	$(CC) $(CFLAGSOPT) -o $@ -c $<

filesys.o: filesys.c
	$(CC) $(CFLAGSOPT) -o $@ -c $<

smartmedia.o: smartmedia.c
	$(CC) $(CFLAGSOPT) -o $@ -c $<

%_3x.o: %.c
	$(CC) $(CFLAGS3X) -o $@ -c $<

%.o: %.c
	$(CC) $(CFLAGS) -c $<

../$(LIBNAME).prc: $(LIBNAME) res.stamp
	$(BUILDPRC) -o $@ driver.def $(LIBNAME) *.bin
	m68k-palmos-objdump --section-headers $(LIBNAME)

../$(LIBNAMEP).prc: libr0000.$(LIBNAMEP).grc res.stamp
	$(BUILDPRC) -o $@ driver.def $(LIBNAMEP) *.bin
	m68k-palmos-objdump --section-headers $(LIBNAMEP)

res.stamp: fsel.rcp fsel.h
	$(PILRC) fsel.rcp .
	touch res.stamp

$(LIBNAME): $(OBJECTS) $(OBJECTS3X)
	$(LD) -o $(LIBNAME) $(OBJECTS) $(OBJECTS3X) $(XTRALIBS) driver.ld

$(LIBNAMEP): $(OBJECTS) $(OBJECTSP)
	$(LD) -o $(LIBNAMEP) $(OBJECTS) $(OBJECTSP) $(XTRALIBS) driver.ld

clean:
	rm -f *.bin *.stamp *.grc *.o $(LIBNAME) $(LIBNAMEP) *~

depend:
	makedepend *.c

# DO NOT DELETE

axxPacDrv.o: axxPacDrv.h ../api/axxPac.h smartmedia.h
errors.o: axxPacDrv.h ../api/axxPac.h errors.h
filesys.o: axxPacDrv.h ../api/axxPac.h smartmedia.h filesys.h
fsel.o: axxPacDrv.h ../api/axxPac.h smartmedia.h errors.h fsel.h
smartmedia.o: axxPacDrv.h ../api/axxPac.h smartmedia.h errors.h
TrapHook.o: axxPacDrv.h ../api/axxPac.h
