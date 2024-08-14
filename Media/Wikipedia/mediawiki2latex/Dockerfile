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

FROM debian:latest as builder

ARG GID=200
ARG UID=1000
ARG USER_NAME="mediawiki2latex"
ARG GROUP_NAME="mediawiki2latex"

ARG BUILD_FOLDER="/tmp"
ARG WEB_DATA="/home/${USER_NAME}/www-work-data"

ARG GIT="https://github.com"
ARG WB2PDF_GIT_FOLDER="wb2pdf-git"
ARG IM_POLICY="/etc/ImageMagick-6/policy.xml"
ARG MEDIAWKI2LATEX_URL="https://git.code.sf.net/p/wb2pdf/git"

MAINTAINER Stanislav Mikhailov (xavetar) <xavetar@proton.me>

WORKDIR ${BUILD_FOLDER}

RUN apt-get update && apt-get -y upgrade \
 && apt-get -y install git make apt-rdepends \
    ghc libghc-x509-dev libghc-pem-dev libghc-regex-compat-dev libghc-http-dev \
    cabal-install libghc-hxt-dev libghc-split-dev libghc-blaze-html-dev \
    libghc-file-embed-dev libghc-hxt-http-dev libghc-regex-pcre-dev \
    libghc-happstack-server-dev libghc-temporary-dev libghc-url-dev \
    libghc-utf8-string-dev libghc-utility-ht-dev libghc-http-conduit-dev \
    libghc-strict-dev libghc-directory-tree-dev libghc-zip-archive-dev \
    libghc-network-uri-dev libghc-tagsoup-dev libghc-word8-dev \
    imagemagick ghostscript calibre latex2rtf libreoffice

RUN apt-get -y install $(apt-rdepends mediawiki2latex | awk '!/Depends:/' | awk '!/mediawiki2latex/' | awk '!/debconf-2.0/' | sed 's|/n| |g')

RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS" />#<policy domain="coder" rights="read|write" pattern="PS" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS2" />#<policy domain="coder" rights="read|write" pattern="PS2" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS3" />#<policy domain="coder" rights="read|write" pattern="PS3" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="EPS" />#<policy domain="coder" rights="read|write" pattern="EPS" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PDF" />#<policy domain="coder" rights="read|write" pattern="PDF" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="XPS" />#<policy domain="coder" rights="read|write" pattern="XPS" />#' ${IM_POLICY}

FROM builder AS mediawiki2latex
RUN git clone "${MEDIAWKI2LATEX_URL}" "${WB2PDF_GIT_FOLDER}" \
 && cd "${WB2PDF_GIT_FOLDER}" \
 && make -j16 \
 && make install \
 && cd .. \
 && rm -rf "${WB2PDF_GIT_FOLDER}"

RUN apt-get -y remove git make apt-rdepends
RUN apt-get clean cache

RUN groupadd -g ${GID} ${GROUP_NAME} \
 && useradd -m -u ${UID} -g ${GROUP_NAME} ${USER_NAME}

USER ${USER_NAME}

WORKDIR ${WEB_DATA}

RUN chcon -R -h ${USER_NAME}:object_r:httpd_sys_content_t ${WEB_DATA}

EXPOSE 8080

ENTRYPOINT ["sh", "-c", "mediawiki2latex -s 8080 > /dev/null 2>&- 1>&- 0>&-"]
