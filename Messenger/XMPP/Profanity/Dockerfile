#
# Copyright 2023 Stanislav Mikhailov (xavetar)
#
# Licensed under the Creative Commons Zero v1.0 Universal (CC0) License.
# You may obtain a copy of the License at
#
#     http://creativecommons.org/publicdomain/zero/1.0/
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the CC0 license is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Build the latest Fedora image
FROM fedora:latest as builder

ARG BUILD_FOLDER="/usr/build"

MAINTAINER Stanislav Mikhailov (xavetar) <xavetar@proton.me>

ENV LANG="en_US.UTF-8"
ENV LANGUAGE="en_US:en"
ENV LC_ALL="en_US.UTF-8"

RUN dnf install -y \
  autoconf autoconf-archive automake expat-devel expect-devel \
  gcc gdk-pixbuf2-devel git glib2-devel glibc-all-langpacks gpgme-devel \
  gtk2-devel libXScrnSaver-devel libcmocka-devel libcurl-devel libgcrypt-devel \
  libmicrohttpd-devel libnotify-devel libotr-devel libsignal-protocol-c-devel \
  libstrophe-devel libtool libuuid-devel make ncurses-devel openssl-devel \
  python3 python3-devel qrencode-devel readline-devel sqlite-devel

WORKDIR ${BUILD_FOLDER}

FROM builder as profanity
RUN git clone --recursive --depth 1 https://github.com/profanity-im/profanity \
 && cd profanity \
 && ./bootstrap.sh \
 && ./configure --prefix=/usr \
 && make -j4 \
 && make install \
 && cd .. \
 && rm -rf profanity

RUN dnf remove -y autoconf autoconf-archive \
    automake gcc git libtool make python3-devel \
 && dnf autoremove \
 && dnf clean all

ENTRYPOINT ["profanity"]