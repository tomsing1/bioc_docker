# DO NOT EDIT FILES CALLED 'Dockerfile'; they are automatically
# generated. Edit 'Dockerfile.in' and generate the 'Dockerfile'
# with the 'rake' command.

# The suggested name for this image is: bioconductor/devel_flow.

FROM bioconductor/devel_core

MAINTAINER dtenenba@fredhutch.org

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    libgsl0-dev libgsl0ldbl libfftw3-dev imagemagick

# flowWorkspace needs a newer version of protobuf than is available via apt-get
ADD https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.gz /tmp/protobuf-2.6.1.tar.gz

RUN cd /tmp && \
    tar -zxf /tmp/protobuf-2.6.1.tar.gz && \
    rm protobuf-2.6.1.tar.gz && \
    cd protobuf-2.6.1 && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install


ADD installFromBiocViews.R /tmp/

RUN R -f /tmp/installFromBiocViews.R

