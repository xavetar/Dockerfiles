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

FROM ubuntu:latest

MAINTAINER Stanislav Mikhailov (xavetar) <xavetar@proton.me>

RUN apt-get update && apt-get -y upgrade

RUN apt-get -y install net-tools tcpdump telnet traceroute curl iperf3 knot-host openssh-client mtr-tiny socat nano vim-tiny nmap iputils-ping

RUN rm -rf /var/lib/apt/lists/*

VOLUME ["/root"]
CMD ["/bin/bash", "-i"]
