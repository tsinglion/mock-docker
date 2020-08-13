## wget http://10.7.10.100/enterprisec/kongzi/release/publish/x86_64/uos-enterprisec-container-20-amd64.tar.xz
## cat uos-enterprisec-container-20-amd64.tar.xz |docker import - uos-uelc20-amd64

FROM uos-uelc20-amd64:latest

## docker 内部的repo源没有激活不能用
ADD ./repo.conf /tmp/repo.conf
ADD ./uelc20-x86_64.cfg /etc/mock/uelc20-x86_64.cfg
ADD ./build-rpm.sh /build-rpm.sh

RUN dnf -c /tmp/repo.conf clean all 														&& \
	dnf -c /tmp/repo.conf -y --setopt="tsflags=nodocs" install mock dnf-plugins-core expect && \
	dnf -c /tmp/repo.conf clean all 														&& \
	rm -rf /var/cache/dnf/																	&& \
	chmod +x /build-rpm.sh																	&& \
 	useradd -u 135 -G mock mockbuild -d /rpmbuild 											&& \
	chmod a+r /etc/mock/*.cfg																&& \
	install -g mock -m 2775 -d /rpmbuild/cache/mock 										&& \
	echo "config_opts['cache_topdir'] = '/rpmbuild/cache/mock'" >> /etc/mock/site-defaults.cfg 

VOLUME ["/rpmbuild"]

CMD ["/build-rpm.sh"]
