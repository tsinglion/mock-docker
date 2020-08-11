## wget http://10.7.10.100/euler/fuyu/release/publish/uos-docker-20-arm64.tar.xz
## docker docker load -i uos-docker-20-arm64.tar.xz

FROM uos-docker-20-arm64:latest

## docker 内部的repo源没有激活不能用
ADD ./repo.conf /tmp/repo.conf
ADD ./build.cfg /etc/mock/build.cfg
ADD ./build-rpm.sh /build-rpm.sh

RUN dnf -c /tmp/repo.conf clean all 														&& \
	dnf -c /tmp/repo.conf -y --setopt="tsflags=nodocs" install mock dnf-plugins-core expect && \
	dnf -c /tmp/repo.conf clean all 														&& \
	rm -rf /var/cache/dnf/																	&& \
	chmod +x /build-rpm.sh																	&& \
 	useradd -u 135 -G mock mockbuild -d /rpmbuild 											&& \
	chmod a+r /etc/mock/*.cfg																&& \
VOLUME ["/rpmbuild"]

CMD ["/build-rpm.sh"]