# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM clusterdock/topology_nodebase:centos6.8

ARG JDK_URL=http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
ARG JCE_URL=http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip
ARG JDK_DIRECTORY=/usr/java/jdk1.8.0_181
ARG PULSAR_URL=http://archive.apache.org/dist/incubator/pulsar/pulsar-2.0.1-incubating/apache-pulsar-2.0.1-incubating-bin.tar.gz
ARG PULSAR_DIRECTORY=/opt/pulsar

RUN yum -y update && \
    yum -y install unzip

# Java setup
RUN wget --no-check-certificate \
        --header "Cookie: oraclelicense=accept-securebackup-cookie" -O /tmp/jdk.tar.gz \
        "${JDK_URL}" && \
    mkdir -p /usr/java && \
    tar xf /tmp/jdk.tar.gz -C /usr/java && \
    rm -f /tmp/jdk.tar.gz

RUN wget --no-check-certificate \
        --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" -O /tmp/unlimited_jce_policy.zip \
        "${JCE_URL}" && \
    unzip -jo -d ${JDK_DIRECTORY}/jre/lib/security /tmp/unlimited_jce_policy.zip && \
    rm -f /tmp/unlimited_jce_policy.zip

# Make the installed Java as default
RUN update-alternatives --install "/usr/bin/java" "java" "${JDK_DIRECTORY}/bin/java" 1
RUN update-alternatives --set java ${JDK_DIRECTORY}/bin/java

# Install Pulsar to ${PULSAR_DIRECTORY}
RUN mkdir -p "${PULSAR_DIRECTORY}" && \
    PULSAR_NAME=$(basename ${PULSAR_URL}) && \
    wget -O "/tmp/${PULSAR_NAME}" "${PULSAR_URL}" && \
    tar -xf "/tmp/${PULSAR_NAME}" --directory "${PULSAR_DIRECTORY}" --strip 1 && \
    rm -f "/tmp/${PULSAR_NAME}"

CMD ["/sbin/init"]
