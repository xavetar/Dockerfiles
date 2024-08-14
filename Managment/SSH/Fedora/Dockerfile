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

FROM fedora:latest

# Installing the SSH Server and Required Packages
RUN dnf -y update \
 && dnf -y install openssh-server \
 && dnf clean all

# Creating an "admin" user and setting a password
RUN useradd -m -d /home/admin -s /bin/bash admin \
 && echo "admin:pass" | chpasswd

# Host key generation
RUN ssh-keygen -A

# Enable password login via SSH
RUN sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Forwarding the port 22 for SSH
EXPOSE 22

# Create and mount local folder /Users/
VOLUME ["/Users/"]

# Run SSH-server
ENTRYPOINT ["/usr/sbin/sshd", "-D"]
