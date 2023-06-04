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

ARG GIT="https://github.com"
ARG WB2PDF_GIT_FOLDER="wb2pdf-git"
ARG IM_POLICY="/etc/ImageMagick-6/policy.xml"
ARG MEDIAWKI2LATEX_URL="https://git.code.sf.net/p/wb2pdf/git"

MAINTAINER Stanislav Mikhailov (xavetar) <xavetar@proton.me>

RUN apt-get update && apt-get -y upgrade

RUN apt-get -y install git make mediawiki2latex

RUN apt-get -y install ghc libghc-x509-dev libghc-pem-dev libghc-regex-compat-dev libghc-http-dev
RUN apt-get -y install cabal-install libghc-hxt-dev libghc-split-dev libghc-blaze-html-dev
RUN apt-get -y install libghc-file-embed-dev libghc-hxt-http-dev libghc-regex-pcre-dev
RUN apt-get -y install libghc-happstack-server-dev libghc-temporary-dev libghc-url-dev
RUN apt-get -y install libghc-utf8-string-dev libghc-utility-ht-dev libghc-http-conduit-dev
RUN apt-get -y install libghc-strict-dev libghc-directory-tree-dev libghc-zip-archive-dev
RUN apt-get -y install libghc-network-uri-dev libghc-tagsoup-dev libghc-word8-dev
RUN apt-get -y install imagemagick ghostscript calibre latex2rtf libreoffice

RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS" />#<policy domain="coder" rights="read|write" pattern="PS" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS2" />#<policy domain="coder" rights="read|write" pattern="PS2" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PS3" />#<policy domain="coder" rights="read|write" pattern="PS3" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="EPS" />#<policy domain="coder" rights="read|write" pattern="EPS" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="PDF" />#<policy domain="coder" rights="read|write" pattern="PDF" />#' ${IM_POLICY}
RUN sed -i 's#<policy domain="coder" rights="none" pattern="XPS" />#<policy domain="coder" rights="read|write" pattern="XPS" />#' ${IM_POLICY}

RUN apt-get -y remove mediawiki2latex

FROM builder AS mediawiki2latex
RUN git clone "${MEDIAWKI2LATEX_URL}" "${WB2PDF_GIT_FOLDER}" \
    && cd "${WB2PDF_GIT_FOLDER}" \
    && make -j16 \
    && make install \
    && cd .. \
    && rm -rf "${WB2PDF_GIT_FOLDER}"

RUN apt-get -y remove git make
RUN apt-get clean cache

EXPOSE 8080

ENTRYPOINT ["mediawiki2latex"]
CMD ["mediawiki2latex", "-s", "8080"]
