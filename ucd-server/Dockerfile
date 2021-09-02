FROM ubuntu:focal

ARG JFROG_UCD_URL
ARG JFROG_API_KEY
ARG APPDIR="/opt/ucd-server"
ARG JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"

ENV APPDIR="${APPDIR}"
ENV PATH="${APPDIR}/bin:${PATH}"
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install wget openjdk-11-jdk curl unzip -y

WORKDIR /tmp

COPY ./files/ibm-ucd-7.0.5.6.1109794.zip ibm-ucd.zip
#RUN curl -H "X-JFrog-Art-Api:${JFROG_API_KEY}" "${JFROG_UCD_URL}" -L -o ibm-ucd.zip && \
RUN unzip ibm-ucd.zip && \
    rm -f ibm-ucd.zip

COPY ./files/install.properties ibm-ucd-install/install.properties
RUN sed "s|{{APPDIR}}|${APPDIR}|g" -i ibm-ucd-install/install.properties

RUN useradd -s /bin/bash -m -d ${APPDIR} ucd && \
    chmod 700 ${APPDIR}

RUN cd ibm-ucd-install && \
    /bin/bash install-server.sh -install-dir ${APPDIR} -silent && \
    chown ucd:ucd -R ${APPDIR} && \
    cd .. && rm -rf ibm-ucd-install

WORKDIR ${APPDIR}
USER ucd

COPY --chown=ucd ./entrypoint.sh .

ENTRYPOINT ["/bin/bash", "entrypoint.sh"]