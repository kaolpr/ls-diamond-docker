FROM centos:7

COPY env-setup.sh /env-setup.sh
COPY license.dat /license.dat

RUN yum update ; \
	yum install -y \
        freetype \
        libSM \
        libXrender \
        fontconfig \
        libXext \
        libXt \
		tcl \
		tcsh \
		perl \
		libXft \
		xorg-x11-fonts-Type1 \
		net-tools \
		libXScrnSaver-1.2.2 \
		libusb-0.1.4 \
		usbutils\
		glibc.i686 \
		libXext-1.3.3-3.el7.i686 \
		libXft-2.3.2-2.el7.i686 \
		libgcc-4.8.5-44.el7.i686

RUN adduser -u 1000 -g 100 diamond

ENTRYPOINT ["/bin/bash"]
