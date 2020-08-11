## wget http://10.7.10.100/euler/fuyu/release/publish/uos-docker-20-arm64.tar.xz
## docker docker load -i uos-docker-20-arm64.tar.xz

FROM uos-docker-20-arm64:latest

## docker 内部的repo源没有激活不能用
ADD ./repo.conf /tmp/repo.conf

ADD ./build.cfg /etc/mock/build.cfg

RUN yum -c /tmp/repo.conf -y --setopt="tsflags=nodocs" update && \
	yum -c /tmp/repo.conf clean all && \
	yum -c /tmp/repo.conf -y --setopt="tsflags=nodocs" install mock dnf-plugins-core expect && \
	yum -c /tmp/repo.conf clean all && \
	rm -rf /var/cache/yum/

#Configure users
RUN useradd -u 1000 -G mock builder && \
	chmod g+w /etc/mock/*.cfg

VOLUME ["/rpmbuild"]

ONBUILD COPY mock /etc/mock

# create mock cache on external volume to speed up build
RUN install -g mock -m 2775 -d /rpmbuild/cache/mock
# RUN echo "config_opts['cache_topdir'] = '/rpmbuild/cache/mock'" >> /etc/mock/site-defaults.cfg

ADD ./build-rpm.sh /build-rpm.sh
RUN chmod +x /build-rpm.sh

CMD ["/build-rpm.sh"]